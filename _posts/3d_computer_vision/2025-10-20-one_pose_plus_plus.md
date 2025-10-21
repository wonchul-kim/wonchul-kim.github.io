---
layout: post
title: OnePose++
category: 3D Computer Vision
tag: [6d pose estimation, 6d]
---


# [OnePose++: Keypoint-Free One-Shot Object Pose Estimation without CAD Models](https://openreview.net/pdf?id=BZ92dxDS3tO)


* The previous feature-matching-based method OnePose 
    
    * a one-shot setting which eliminates the need for CAD models or object-specific training. 
    
    * relies on detecting repeatable image keypoints and is thus prone to failure on low-textured objects.

        * low-textured objects는 keypoint-based SfM를 통하여 point cloud로 재구성 하기 힘들기 때문

* This paper propose a keypoint-free pose estimation pipeline to remove the need for repeatable keypoint detection. 

    * Built upon the detector-free feature matching method **LoFTR**, we devise a new keypoint-free SfM method to reconstruct a semi-dense point-cloud model for the object. 

    * Given a query image for object pose estimation, a 2D-3D matching network directly establishes 2D-3D correspondences between the query image and the reconstructed point-cloud model without first detecting keypoints in the image. 

        * 기존에는 `keypoint`를 추출하고 이에 대한 `descriptor`를 정의하고, 기준이 되는 `descriptor`와 비교하여 correpondences를 찾는다.
            
            > `descriptor`: `keypoint`가 이미지 내에서 얼마나 독특한지를 나타내며, 이는 `keypoint` 주변의 픽셀 강도 변화, gradient 방향, 색상 분포 등 시각적 패턴을 일련의 숫자(보통 64차원, 128차원 또는 그 이상)로 압추한다. 

        * **OnePose**는 query image와 3D 모델(point cloud)를 직접 입력으로 받아, 별도의 keypoint 검출 단계없이 2D 픽셀과 3D 점 사이의 correpondences를 네트워크가 직접 학습하고 예측한다.

            * 이는 픽셀 또는 작은 영역의 특징을 3D 공간의 위치(점군)와 바로 연결하는 방식으로 기존처럼 검출된 `keypoint`에 의존하지 않고 이미지의 전체 정보를 활용할 수 있다는 장점이 있음.



            


## Intro.

> It uses centers of regular grids on a left image as “keypoints”, and extracts sub-pixel accuracy matches on the right image in a coarse-to-fine manner. However, this two-viewdependent nature leads to inconsistent “keypoints” and fragmentary feature tracks, which go against the preference of modern SfM systems. Therefore, keypoint-free feature matching cannot be directly applied to OnePose for object pose estimation. 


* keypoint-free matching for one-shot object pose estimation: two-stage pipeline for reconstructing a 3D structure, striving for both accuracy and completeness. 

* For testing, we propose a sparse-to-dense 2D-3D matching network that efficiently establishes accurate 2D-3D correspondences for pose estimation, taking full advantage of our keypoint-free design. We disassemble the coarse-to-fine structure of LoFTR and integrate them into our reconstruction pipeline. 
    
    * In the coarse reconstruction phase, we use less accurate yet repeatable LoFTR coarse correspondences to construct consistent feature tracks for SfM and yield an inaccurate but complete semi-dense point cloud. 
    
    * Then, our novel refinement phase optimizes the initial point cloud by refining “keypoint” locations in coarse feature tracks to sub-pixel accuracy. 


* At test time, we draw inspiration from the sparse-to-dense matching strategy in visual localization [12], and further adapt it to direct 2D-3D matching in a coarse-to-fine manner for efficiency. 

* Additionally, we use self- and cross-attention to model long-range dependencies required for robust 2D-3D matching and pose estimation of complex real-world objects, which usually contain repetitive patterns or low-textured regions.


* Contributions:
    * A keypoint-free SfM method for semi-dense reconstruction of low-textured objects.

    * A sparse-to-dense 2D-3D matching network for accurate object pose estimation.


#### CAD-Model-Free Object Pose Estimation. 

* NeRF-Pose [24] reconstructs a neural representation NeRF [36] for an object first and train an object coordinate regression network for pose estimation. 
    
    * These methods are not generalizable to unseen objects. 

* The recently proposed Gen6D [33] and OnePose [48] only require a set of reference images with annotated poses to estimate object poses and can generalize to unseen objects. 
    
    * Gen6D uses detection and retrieval to initialize the pose of a query image and then refine it by regressing the pose residual. 
    
    * However, Gen6D requires an accurately detected 2D bounding box for pose initialization and struggles with occlusion scenarios. 
    

> Some of previous methods either rely on the ratio test in the matching stage [35, 14, 46] or leverage prioritized hypothesis testing in the outlier filtering stage [9, 17] to reject ambiguous matches.
> 1. ratio test: keypoint `descriptor`를 비교하여 가장 유사한 매칭 쌍 $\text{D}_1$과 두 번째로 유사한 매칭 쌍 $\text{D}_2$를 찾은 후, $\text{D}_1$과 $\text{D}_2$ 간의 유사도 비율($\text{D}_1 / \text{D}_2$)이 특정 임계값보다 작을 때만 매칭을 유효하다고 판단하는 방법
>       * 목적: 가장 가까운 매칭이 두 번째로 가까운 매칭보다 충분히 독특할 때만 채택하여, 모호한(ambiguous) 매칭(즉, 여러 곳에 비슷한 기술자가 있는 경우)을 매칭 단계에서 바로 거부
> 2. prioritized hypothesis testing: RANSAC(RANdom SAmple Consensus)과 같은 아웃라이어 제거 알고리즘을 사용할 때, 단순히 무작위로 점 쌍을 선택하여 자세(pose) 가설을 세우는 것이 아니라, 더 신뢰할 수 있을 것 같은 매칭 쌍을 우선적으로 선택하여 가설을 세우고 검증하는 방법
>       * 목적: 초기에 정확한 매칭 쌍(인라이어, inlier)을 사용하여 자세 가설의 정확도를 높임으로써, 나중에 잘못된 매칭(outlier)이 자세 계산에 미치는 영향을 아웃라이어 필터링 단계에서 최소화하고 제거



> **Structure from Motion and Visual Localization**
> Visual localization estimates camera poses of query images relative to a known scene structure. The scene structure is usually reconstructed with Structure-from-Motion (SfM) [44], relying on the feature matching methods [34, 6, 62, 25, 41, 13]. The localization problem is then solved by finding 2D-3D correspondences between the 3D scene model and a query image. HLoc [40] scales up visual localization in a coarse-to-fine manner. It is based on image retrieval and establishes 2D-3D correspondences by lifting 2D-2D matches between query images and retrieved database images to 3D space. However, HLoc is not suitable for our setting since it is slow during pose estimation because it depends on 2D-2D matching of multiple image pairs as the proxy to locate one query image. Our framework is more relevant to the previous visual localization methods which are based on efficient direct 2D-3D matching [43, 26, 49, 32, 3, 60, 27, 5, 7, 50, 42, 12]. To boost matching efficiency between the large-scale point cloud and query images, some of them narrow the searching range by leveraging the priors [43, 26, 5] or compress 3D models by quantizing features [43, 32, 42]. However, these strategies contribute little to the disambiguation of features. They often use priors [43, 32, 27, 42] or geometric verification [49, 3, 60, 50] in the outlier filtering stage to cope with the challenges from low-textured regions or repetitive patterns. In contrast, our method works on the 2D-3D matching phase but focuses on disambiguating features before matching. Our idea is to directly disambiguate and augment dense features by implicitly encoding their spatial information and relations with other features in a learning-based manner.

> Our keypoint-free SfM framework is related to SfM refinement methods PatchFlow [8] and PixSfM [31]. They improve keypoint-based SfM for more accurate 3D reconstructions by refining inaccurately-detected sparse local features with local patch flow [8] or dense feature maps [31]. Different from them, we leverage fine-level matching with Transformer [53] to refine the 2D locations of coarse feature tracks and then optimize the 3D model with geometric error. [57] also works on keypoint-free SfM. However, it refines the coarse matches by the keypoint relocalization, which is single-view dependent and faces the issue of inaccurate keypoint detection. In contrast, our refinement leverage two-view patches to find accurate matches. Note that there are also methods proposed by keypoint-free matchers [47, 62] to adapt themselves for SfM. They either round matches to grid level [47] or merge matches within a grid to the average location [62] to obtain repeatable “keypoints” for SfM. However, all these approaches trade-off point accuracy for repeatability. On the contrary, our framework obtains repeatable features while preserving the sub-pixel matching accuracy by the refinement phase.

## References
- [OnePose++: Keypoint-Free One-Shot Object Pose Estimation without CAD Models](https://openreview.net/pdf?id=BZ92dxDS3tO)
- [LoFTR: Detector-Free Local Feature Matching with Transformers](https://arxiv.org/pdf/2104.00680)
- [Sparse-to-Dense Hypercolumn Matching for Long-Term Visual Localization](https://arxiv.org/pdf/1907.03965)
