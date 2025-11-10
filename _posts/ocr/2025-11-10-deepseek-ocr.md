---
layout: post
title: DeepSeek-OCR: Contexts Optical Compression
category: OCR
tag: [deepseek-ocr]
---


# [DeepSeek-OCR: Contexts Optical Compression](https://arxiv.org/pdf/2510.18234)

- **optical 2D mapping**을 활용한 `long contexts`를 이미지 단위(`vision token`)로 압축해서 해석한다는 개념에 대한 가능성을 제시함

- **DeepEncoder** serves as the core engine, designed to maintain low activations under high-resolution input while achieving high compression ratios to ensure an optimal and manageable number of vision tokens. 

- **LLM**분야에서의 **historical long-context compression**과 **memory forgetting mechanisms**의 영역에서의 해결책으로서의 가능성을 보여줌


## 1. Intro.

- Current Large Language Models (LLMs) face significant computational challenges when processing long textual content due to quadratic scaling with sequence length. 

- 이에 대하여, 
    
    - textual information을 효율적으로 압축하는 수단으로서 `visual modality` 제시

    - `optical compression`이라고 정의하며, `vision tokens`을 단위로 하여 `compression ratio`를 조절할 수 있음

    - 이로서 **vision encoder**가 **LLM**의 textual information을 얼마나 효율적으로 처리할 수 있는지에 대한 가능성 제시하며, 기존의 **VQA**와 대비됨

- **DeepEncoder**, a novel architecture that maintains low activation memory and minimal vision tokens even with high-resolution inputs. 

    - It serially connects window attention and global attention encoder components through a 16× convolutional compressor. 
    
    - This design ensures that the window attention component processes a large number of vision tokens, while the compressor reduces vision tokens before they enter the dense global attention component, achieving effective memory and token compression.

- **DeepSeek3B-MoE [19, 20]**



## 2. Related Works

#### 2.1. Typical Vision Encoders in VLMs

**dual-tower architecture represented by Vary [36]**, which utilizes parallel SAM [17] encoder to increase visual vocabulary parameters for high-resolution image processing.

- While offering controllable parameters and activation memory, this approach suffers from significant drawbacks: 
    
    - it requires dual image preprocessing that complicates deployment and makes encoder pipeline parallelism challenging during training. 
    
**tile-based method exemplified by InternVL2.0 [8]**, which processes images by dividing them into small tiles for parallel computation, reducing activation memory under high-resolution settings. 

- Although capable of handling extremely high resolutions, this approach has notable limitations due to its typically low native encoder resolution (below 512×512), causing large images to be excessively fragmented and resulting in numerous vision tokens. 


**adaptive resolution encoding represented by Qwen2-VL [35]**, which adopts the NaViT [10] paradigm to directly process full images through patch-based segmentation without tile parallelization. 

- While this encoder can handle diverse resolutions flexibly, it faces substantial challenges with large images due to massive activation memory consumption that can cause GPU memory overflow, and sequence packing requires extremely long sequence lengths during training. 

- Long vision tokens will slow down both prefill and generation phases of inference. 

#### 2.2. End-to-end OCR Models

**Nougat [6]** first employs end-to-end framework for academic paper OCR on arXiv, demonstrating the potential of models in handling dense perception tasks. 

**GOT-OCR2.0 [38]** expands the scope of OCR2.0 to include more synthetic image parsing tasks and designs an OCR model with performance-efficiency trade-offs, further highlighting the potential of end-to-end OCR researches. 

**general vision models such as Qwen-VL series [35], InternVL series [8]**, and many their derivatives continuously enhance their document OCR capabilities to explore dense visual perception boundaries. 

하지만, ***how many vision tokens are at least needed for decoding?*** -> "a picture is worth a thousand words."


## Methodology

#### 3.1 Architecture 

<img src='/assets/ocr/deepseek_ocr/arch.png'>

- **DeepEncoder** is approximately 380M in parameters, mainly composed of an 80M SAM-base [17] and a 300M CLIP-large [29] connected in series. 

- The decoder adopts a 3B MoE [19, 20] architecture with 570M activated parameters. 


#### 3.2. DeepEncoder

covers the below:

- 1.Capable of processing high resolutions

- 2.Low activation at high resolutions

- 3.Few vision tokens

- 4.Support for multiple resolution inputs

- 5. Moderate parameter count


#### 3.2.1. Architecture of DeepEncoder

**DeepEncoder** mainly consists of two components: 

- a visual perception feature extraction component dominated by window attention

- a visual knowledge feature extraction component with dense global attention

To benefit from the pretraining gains of previous works, we use **SAM-base (patch-size 16)** and **CLIP-large** as the main architectures for the two components respectively. 

- For **CLIP**, we remove the first patch embedding layer since its input is no longer images but output tokens from the previous pipeline. 

Between the two components, we borrow from Vary [36] and use a 2-layer convolutional module to perform 16× downsampling of vision tokens. 

Each convolutional layer has a kernel size of 3, stride of 2, padding of 1, and channels increase from 256 to 1024. 

Assuming we input a 1024×1024 image, the DeepEncoder will segment it into 1024/16×1024/16=4096 patch tokens. 

Since the first half of encoder is dominated by window attention and only 80M, the activation is acceptable. 

Before entering global attention, the 4096 tokens go through the compression module and the token count becomes 4096/16=256, thus making the overall activation memory controllable.

#### 3.2.2. Multiple resolution support

Suppose we have an image with 1000 optical characters and we want to test how many vision tokens are needed for decoding. 

This requires the model to support a variable number of vision tokens. That is to say the DeepEncoder needs to support multiple resolutions.

We meet the requirement aforementioned through dynamic interpolation of positional encodings, and design several resolution modes for simultaneous model training to achieve the capability of a single DeepSeek-OCR model supporting multiple resolutions. 

As shown in Figure 4, DeepEncoder mainly supports two major input modes: native resolution and dynamic resolution. Each of them contains multiple sub-modes.

Native resolution supports four sub-modes: Tiny, Small, Base, and Large, with corresponding resolutions and token counts of 512×512 (64), 640×640 (100), 1024×1024 (256), and 1280×1280 (400) respectively. Since Tiny and Small modes have relatively small resolutions, to avoid
wasting vision tokens, images are processed by directly resizing the original shape. For Base
and Large modes, in order to preserve the original image aspect ratio, images are padded to
the corresponding size. After padding, the number of valid vision tokens is less than the actual
number of vision tokens, with the calculation formula being:
𝑁𝑣𝑎𝑙𝑖𝑑 = ⌈𝑁𝑎𝑐𝑡𝑢𝑎𝑙 × [1 − ( (𝑚𝑎𝑥(𝑤, ℎ) − 𝑚𝑖𝑛(𝑤, ℎ))/(𝑚𝑎𝑥(𝑤, ℎ)))]⌉ (1)
where 𝑤 and ℎ represent the width and height of the original input image


Dynamic resolution can be composed of two native resolutions. For example, Gundam
mode consists of n×640×640 tiles (local views) and a 1024×1024 global view. The tiling method
following InternVL2.0 [8]. Supporting dynamic resolution is mainly for application considerations, especially for ultra-high-resolution inputs (such as newspaper images). Tiling is a form of
secondary window attention that can effectively reduce activation memory further. It’s worth
noting that due to our relatively large native resolutions, images won’t be fragmented too much
under dynamic resolution (the number of tiles is controlled within the range of 2 to 9). The
vision token number output by the DeepEncoder under Gundam mode is: 𝑛 × 100 + 256, where
𝑛 is the number of tiles. For images with both width and height smaller than 640, 𝑛 is set to 0,
i.e., Gundam mode will degrade to Base mode.
Gundam mode is trained together with the four native resolution modes to achieve the goal
of one model supporting multiple resolutions. Note that Gundam-master mode (1024×1024 local
views+1280×1280 global view) is obtained through continued training on a trained DeepSeekOCR model. This is mainly for load balancing, as Gundam-master’s resolution is too large and
training it together would slow down the overall training speed.

#### 3.3. The MoE Decoder
Our decoder uses the DeepSeekMoE [19, 20], specifically DeepSeek-3B-MoE. During inference,
the model activates 6 out of 64 routed experts and 2 shared experts, with about 570M activated
parameters. The 3B DeepSeekMoE is very suitable for domain-centric (OCR for us) VLM
research, as it obtains the expressive capability of a 3B model while enjoying the inference
efficiency of a 500M small model.
The decoder reconstructs the original text representation from the compressed latent vision
tokens of DeepEncoder as:
𝑓dec : R
𝑛×𝑑latent → R
𝑁×𝑑text ; Xˆ = 𝑓dec (Z) where 𝑛 ≤ 𝑁 (2)
where Z ∈ R𝑛×𝑑latent are the compressed latent(vision) tokens from DeepEncoder and Xˆ ∈ R𝑁×𝑑text
is the reconstructed text representation. The function 𝑓dec represents a non-linear mapping
that can be effectively learned by compact language models through OCR-style training. It
is reasonable to conjecture that LLMs, through specialized pretraining optimization, would
demonstrate more natural integration of such capabilities.

#### 3.4. Data Engine
We constructe complex and diverse training data for DeepSeek-OCR, including OCR 1.0 data,
which mainly consists of traditional OCR tasks such as scene image OCR and document OCR;
OCR 2.0 data, which mainly includes parsing tasks for complex artificial images, such as
common charts, chemical formulas, and plane geometry parsing data; General vision data,
which is mainly used to inject certain general image understanding capabilities into DeepSeekOCR and preserve the general vision interface.

## References

- [DeepSeek-OCR: Contexts Optical Compression](https://arxiv.org/pdf/2510.18234)

- [DeepSeek-OCR github](https://github.com/deepseek-ai/DeepSeek-OCR)
