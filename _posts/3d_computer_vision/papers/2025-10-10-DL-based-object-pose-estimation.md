---
layout: post
title: Deep Learning-Based Object Pose Estimation A Comprehensive Survey
category: 3D Computer Vision
tag: [3d computer vision, survey]
---


# [Deep Learning-Based Object Pose Estimation A Comprehensive Survey](https://arxiv.org/pdf/2405.07801)

## Intro.
* In the pre-deep learning era, many hand-crafted featurebased approaches such as SIFT [8], FPFH [9], VFH [10], and
Point Pair Features (PPF) [11], [12], [13], [14] were designed for object pose estimation


* Deep learning-based object pose estimation methods can be divided into **instance-level**, **category-level**, and **unseen object methods** according to the problem formulation. 

    <img src='/assets/3d_computer_vision/dl_based_object_pose_estimation/pose_estimation.png'>


    * Instance-level methods can only estimate the pose of specific object instances on which they are trained. 
        
        * Instance-level methods can be further divided into correspondence-based, template-based, voting-based, and regression-based methods. Since instance-level methods are trained on instance-specific data, they can estimate pose with high precision for the given object instances

        * many instance-level methods [18], [21] require CAD models of the objects. 

    * Category-level methods can infer intra-class unseen instances rather than being limited to specific instances in the training data. 
            
        * category-level methods [23], [24], [25], [26], [27] can be divided into shape prior-based and shape prior-free methods. 

        * still need to collect and label extensive training data for each object category.

    * Unseen object pose estimation methods [1], [3], [28], [29], [30], have stronger generalization ability and can handle object categories not encountered during training.

        * classified into CAD model-based and manual reference view-based methods. 
        
        * generalized to unseen objects without retraining. Nevertheless, they still need to obtain the object CAD model or annotate a few reference images of the object.


* encompasses all problem formulations, including instance-level, category-level, and unseen object pose estimation,

* discuss different domain training paradigms, application areas, evaluation metrics, and benchmark datasets, as well as report the performance of state-of-the-art methods on these benchmarks, aiding readers in selecting suitable methods for their applications. 

* highlight prevailing trends and discuss their strengths and weaknesses, as well as identify key challenges
and promising avenues for future research. 

<img src='/assets/3d_computer_vision/dl_based_object_pose_estimation/taxonomy.png'>

#### Contributions
    
* covers popular input data modalities (RGB images, depth images, RGBD images), the different degrees of freedom (3DoF, 6DoF, 9DoF) in output poses, object properties (rigid, articulated) for the task of pose estimation as well as tracking. 

* discuss different domain training paradigms, inference modes, application areas, evaluation metrics, and benchmark datasets as well as report the performance of existing SOTA methods on these benchmarks

* highlight popular trends in the evolution of object pose estimation techniques over the past decade and discuss their strengths and weaknesses.



## DATASETS AND METRICS

<img src='/assets/3d_computer_vision/dl_based_object_pose_estimation/overview_datasets.png'>

### Datasets for Instance-Level Methods

Since the BOP Challenge datasets [36] are currently the most popular datasets for the evaluation of instance-level
methods, we divide the instance-level datasets into BOP Challenge and other datasets for overview.


#### BOP Challenge Datasets

* Linemod Dataset (LM) [37] 
    
    * 15 RGBD sequences containing annotated RGBD images with ground-truth 6DoF object poses, object CAD models, 2D bounding boxes, and binary masks. 
    
    * approximately 15% of images from each sequence are allocated for training, with the remaining 85% reserved for testing.
    
    * challenging scenarios with cluttered scenes, texture-less objects, and varying lighting conditions
    
* Linemod Occlusion Dataset (LM-O) [39]: extension of the LM dataset [37]

    * specifically designed to evaluate the performance in occlusion scenarios. 
    
    * 1214 RGBD images from the basic sequence in the LM dataset for 8 heavily occluded objects. 

#### Others

### Datasets for Category-Level Methods

#### Rigid Objects Datasets

#### Articulated Objects Datasets

### Datasets for Unseen Methods



### Metrics 


## INSTANCE-LEVEL OBJECT POSE ESTIMATION

### Correspondence-Based Methods

* identify correspondences between the input data and the given complete object CAD model

* Correspondence-based methods can be divided into sparse and dense correspondences

* Sparse correspondence-based methods involve detecting object keypoints in the input image or point cloud to establish 2D-3D or 3D3D correspondences between the input data and the object CAD model, followed by the utilization of the Perspectiven-Point (PnP) algorithm [73] or least square method to determine the object pose. 

* Dense correspondence-based methods aim to establish dense 2D-3D or 3D-3D correspondences, ultimately leading to more accurate object pose estimation. For the RGB image, they leverage every pixel or multiple patches to generate pixel-wise correspondences, while for the point cloud, they use the entire point cloud to find point-wise correspondences.



## Applications

### Robotic Manipulation

#### Instance-Level Manipulation

To tackle the challenge of annotating real data during training, many works utilize synthetic data for training as it is easy to acquire and annotate [312], [313]. At the same time, synthetic data can simulate various scenes and environmental changes, thus helping to improve the adaptability of robotic manipulation. Li et al. 
[314] used a large-scale
synthetic dataset and a small-scale weakly labeled real-world
dataset to reduce the difficulty of system deployment. Additionally, Chen et al. [315] proposed an iterative self-training
framework, using a teacher network trained on synthetic
data to generate pseudo-labels for real data. Meanwhile, Fu
et al. [316] trained only on synthetic images based on physical
rendering. One of the critical challenges of synthetic data is
bridging the gap with reality, which Tremblay et al. [317]
addressed by combining domain randomization with real
data.
Handling stacked occlusion scenes is another significant challenge, especially in industrial automation and logistics. In these scenarios, robots must accurately identify
and localize objects stacked on each other, which requires
an effective process of occluded objects and accurate pose
estimation. Dong et al. [318] argued that the regression poses
of points from the same object should tightly reside in the
pose space. Therefore, these points can be clustered into
different instances, and their corresponding object poses can
be estimated simultaneously. This method can handle severe
object occlusion. Moreover, Zhuang et al. [319] established
an end-to-end pipeline to synchronously regress all potential
object poses from an unsegmented point cloud. Most recently, Wada et al. [320] proposed a system that fully utilized
identified accurate object CAD models and non-parametric
reconstruction of unrecognized structures to estimate the
occluded objects pose in real-time.
Low-textured objects lack object surface texture information, making robotic manipulation challenging. Therefore,
Zhang et al. [321] proposed a pose estimation method for
texture-less industrial parts. Poor surface texture and brightness make it challenging to compute discriminative local
appearance descriptors. This method achieves more accurate
results by optimizing the pose in the edge image. In addition,
Chang et al. [322] carried out transparent object grasping
by estimating the object pose using a proposed model-free
method that relies on multiview geometry. In agricultural
scenes, Kim et al. [323] constructed an automated data collection scheme based on a 3D simulator environment to achieve
three-level ripen


## References

- [Deep Learning-Based Object Pose Estimation A Comprehensive Survey](https://arxiv.org/pdf/2405.07801)
- [Awesome-Object-Pose-Estimation](https://github.com/CNJianLiu/Awesome-Object-Pose-Estimation)