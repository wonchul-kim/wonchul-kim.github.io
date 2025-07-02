---
layout: post
title: Tracking
category: Tracking
tag: [tracking]
---

# [Focusing on Tracks for Online Multi-Object Tracking](https://openaccess.thecvf.com/content/CVPR2025/papers/Shim_Focusing_on_Tracks_for_Online_Multi-Object_Tracking_CVPR_2025_paper.pdf)


* current state-of-the-art methods mainly rely on a global optimization technique and multi-stage cascade association strategy
    * those approaches often overlook the specific characteristics of assignment task in MOT and useful detection results that may represent occluded objects


* **Track-Perspective-Based Association (TPA)** and **Track-Aware Initialization (TAI)**

    * The **TPA** strategy associates each track with the most suitable detection result by choosing the one with the minimum distance from all available detection results in a track-perspective manner.
        
        * framing the data association problem in MOT as selecting the optimal match for each track would be more appropriate than solely minimizing global costs, particularly in scenarios involving occlusions
    
    * **TAI** precludes the generation of spurious tracks in the track-aware aspect by suppressing track initialization of detection results that heavily overlap with current active tracks and more confident detection results. 
        
        * utilizes all available detection results, including highconfidence and low-confidence detection results and even high-confidence detection results that are discarded during non-maximum suppression (NMS), while focusing on the local perspective of each track  
        
        * Additionally, we introduce Track-Aware Initialization (TAI), which selectively initializes new tracks only with the most feasible detected boxes among the redundant candidates.  
        

* More specifically, our TPA compares every pair of tracks and detection results simultaneously through a single joint association stage to improve the utilization of every candidate. Then, it iteratively associates tracks and detection results that present the minimum distance while prioritizing local matching precision, ensuring each track is matched with the most appropriate detection result in each track-perspective manner. 

* Meanwhile, TAI excludes detection results that significantly overlap with active tracks and other more confident detection results from the set of track initialization candidates, preventing spurious tracks and contributing to a more reliable tracking process. 



## Process to track

1. Detection 

2. FastReID

3. Track

    - AFLink

    - CMC: global motion compensation model


## References

-  [Focusing on Tracks for Online Multi-Object Tracking](https://openaccess.thecvf.com/content/CVPR2025/papers/Shim_Focusing_on_Tracks_for_Online_Multi-Object_Tracking_CVPR_2025_paper.pdf) [code](https://github.com/kamkyu94/TrackTrack/tree/main)