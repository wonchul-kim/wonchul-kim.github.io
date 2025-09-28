---
layout: post
title: Dinov2 Embeddings
category: Computer Vision
tag: [dinov2, embedding]
---


# Embeddings from Foundation Models

## Dinov2

#### Embedding 종류

| 종류                                    | 설명                                                      | 주요 용도                                          |
| ------------------------------------- | ------------------------------------------------------- | ---------------------------------------------- |
| **\[CLS] Token Embedding**            | 입력 이미지 전체를 요약하는 1개의 벡터 (`[CLS]`)                        | 🔹 classification, zero-shot transfer, retrieval                                      |
| **Patch Embeddings**                  | 이미지가 patch 단위로 분할되어 각 patch에 대해 독립적인 임베딩 제공             | 🔹 Dense prediction (e.g., segmentation), attention map       |
| **Last Hidden State**                 | 모든 토큰(`N` patches + 1 `[CLS]`)에 대한 마지막 레이어 임베딩          | 🔹 다양한 downstream task의 feature 입력             |
| **Intermediate Layer Embeddings**     | 마지막 레이어가 아닌 중간 layer에서의 표현                              | 🔹 시각적 feature 해석, multi-layer feature                              |
| **Projected Embedding (head output)** | linear or MLP head를 통과한 임베딩 (선택적)                       | 🔹 supervised fine-tuning, classification head |
| **Student vs. Teacher Embeddings**    | DINO 구조 특성상 teacher와 student가 있음 → 두 네트워크의 임베딩 모두 사용 가능 | 🔹 knowledge distillation, representation stability                      |


### Embedding dimension

| 모델              | \[CLS] 차원 | patch 개수    | patch embedding 차원 |
| --------------- | --------- | ----------- | ------------------ |
| DINOv2-ViT-S/14 | 384       | 14×14 = 196 | 384                |
| DINOv2-ViT-B/14 | 768       | 196         | 768                |
| DINOv2-ViT-L/14 | 1024      | 196         | 1024               |
| DINOv2-ViT-G/14 | 1536      | 196         | 1536               |

### How to get embedding

#### `facebookresearch/dinov2`


* 모델 요약도 
    ```
        ┌──────────────┐
        │ Input Image  │
        └──────┬───────┘
                ▼
        ┌────────────────────┐
        │ Patch Embedding    │
        └────────┬───────────┘
                ▼
        ┌────────────────────┐
        │ Transformer Blocks │
        └────────┬───────────┘
                ▼
            [CLS] + Patch
                ▼
        ┌──────────────┐
        │ x_prenorm    │ ← LayerNorm 전 전체
        └────────┬─────┘
                ▼
        ┌────────────────────┐
        │ LayerNorm          │
        └────────┬───────────┘
                ▼
        ┌──────────────┬───────────────┐
        │ x_norm_cls   │ x_norm_patch │
        └──────────────┴───────────────┘
    ```


* `outputs` (`dict`)
    ```python
        outputs = model.forward_features(input_tensor)
    ```


    | key                               | 설명                                     | LayerNorm 여부 | 대표 사용 목적                             | 사용 예시                  |
    | ------------------------------- | -------------------------------------- | ------------ | ------------------------------------ | ---------------------- |
    | **`x_norm_clstoken`**           | `[CLS]` token (LayerNorm 이후)           | ✅ 있음         | **이미지 전체 요약** 표현                     | 🔹 classification, retrieval, clustering, clip-like VLMs      |
    | **`x_norm_patchtokens`**        | 모든 patch token (LayerNorm 이후)          | ✅ 있음         | **Dense feature map**                | 🔹 segmentation, detection, captioning (VLMs)        |
    | **`x_prenorm`**                 | `[CLS] + patch tokens`, LayerNorm 적용 전 | ❌ 없음         | **Transformer 내부 (raw output) 분석**, | 🔹 attention weight 분석, residual connection, intermediate feat. |
    | **`masks`**                     | masking 여부 표시 (학습 시 사용)                | —            | **masked autoencoding 학습용**          | 🔹 DINOv2 pretraining, maskd fine-tuning  |


    * `x_norm_clstoken`: (bs, emb_size)
    * `x_norm_patchtoken`: (bs, num_patch, emb_size)
    * `x_prenorm`: (bs, num_patch + 1, emb_size)
    * `masks`: (bs, emb_size)

* **mask-aware forward pass**

    | 용도                             | 설명                                   |
    | ------------------------------ | ------------------------------------ |
    | Pre-training (self-supervised) | 일부 patch를 mask하고, 나머지로 그걸 예측하도록 학습   |
    | Fine-tuning에서 일부 영역 무시         | 특정 영역을 dropout/mask하여 regularization |
    | Dense prediction 학습            | 특정 위치를 의도적으로 무시하거나 masking해서 학습 안정화  |


    ```python
        def forward_features(self, x, masks=None):
            if isinstance(x, list):
                return self.forward_features_list(x, masks)

            x = self.prepare_tokens_with_masks(x, masks)

            for blk in self.blocks:
                x = blk(x)

            x_norm = self.norm(x)
            return {
                "x_norm_clstoken": x_norm[:, 0],
                "x_norm_regtokens": x_norm[:, 1 : self.num_register_tokens + 1],
                "x_norm_patchtokens": x_norm[:, self.num_register_tokens + 1 :],
                "x_prenorm": x,
                "masks": masks,
            }
    ```

    ```python
        def prepare_tokens_with_masks(self, x, masks=None):
        B, nc, w, h = x.shape
        x = self.patch_embed(x) # x: (bs, num_patches, emb_size)
        if masks is not None:
            # self.mask_token: zeros of (bs, emb_size)
            x = torch.where(masks.unsqueeze(-1), self.mask_token.to(x.dtype).unsqueeze(0), x)

        x = torch.cat((self.cls_token.expand(x.shape[0], -1, -1), x), dim=1)
        x = x + self.interpolate_pos_encoding(x, w, h)

        if self.register_tokens is not None:
            x = torch.cat(
                (
                    x[:, :1],
                    self.register_tokens.expand(x.shape[0], -1, -1),
                    x[:, 1:],
                ),
                dim=1,
            )

        return x
    ```

    * `masks`는 **patch-level mask** 정보를 담고 있는 `(B, N) shaped boolean tensor`입니다.

        * `x = torch.where(masks.unsqueeze(-1), self.mask_token.to(x.dtype).unsqueeze(0), x)`
            ```python
                x = torch.where(
                    masks.unsqueeze(-1),                   # shape: (B, N, 1)
                    self.mask_token.unsqueeze(0),         # shape: (1, 1, D) ← broadcast 됨
                    x                                      # shape: (B, N, D)
                )
            ```

        * `x`는 `mask`된 위치는 `mask_token`이 적용되고, 아닌 곳은 원래의 `patch`값이 적용됨



    * 이 구조는 BERT-style masking과 유사하게 작동하여 self-supervised pretraining의 핵심 역할


* `Resister token`

    ```txt
    [CLS] + [REG_1] + [REG_2] + ... + [REG_K] + patch_1 + patch_2 + ...
    ```
    * 위와 같이 embedding에 포함되고, 학습 가능한 token임 (`nn.Parameter`)
    * `num_register_tokens`로 지정

    | 목적                       | 설명                                  | 사용 예시                                          |
    | ------------------------ | ----------------------------------- | ---------------------------------------------- |
    | **Feature collector**    | Patch들이 가진 정보를 "받아들이는 중간 토큰" 역할     | Slot Attention (object-centric), DINOv1-early  |
    | **Auxiliary task**       | Reg token에 MLP head를 붙여서 별도 loss 학습 | e.g., color prediction, contrastive loss       |
    | **Prompt-like role**     | 특정 task에 특화된 instruction 역할         | Meta-Learning, Prompt Tuning                   |
    | **Knowledge bottleneck** | 여러 토큰의 정보를 **압축해서 정리**              | Object-centric learning, causal token learning |


    * 실제 사용 예: Slot Attention (Locatello et al., 2020)

        * 초기 slot token (reg) 들을 transformer에 넣음

        * 이 token들이 전체 이미지의 여러 object에 해당하는 정보를 "받아들임"

        * 이후 이 token들만 decoding

        * DINOv2에서 유사한 걸 하려면 reg_token에 head를 붙여서 이렇게 사용:
            ```python
                x = model.forward_features(x)
                reg_feat = x["x_norm_regtokens"]  # shape: (B, K, D)
                logits = MLP_head(reg_feat)       # auxiliary head
                loss = auxiliary_loss(logits)
            ```

    * 하지만, **Dinov2**에서는 사용하지 않음
