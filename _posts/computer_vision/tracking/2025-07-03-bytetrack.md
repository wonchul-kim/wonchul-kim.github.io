---
layout: post
title: ByteTrack
category: Tracking
tag: [tracking]
---

# [ByteTrack: Multi-Object Tracking by Associating Every Detection Box](https://arxiv.org/pdf/2110.06864)

<img src='/assets/computer_vision/tracking/bytetrack/bytetrack_performance1.png'>

<img src='/assets/computer_vision/tracking/bytetrack/bytetrack_performance2.png'>


* ***occluded objects***, which are simply thrown away, bring non-negligible true object missing and fragmented trajectories.

* to solve this problem, they associate almost every detection box instead of only the high score ones.

* For the low score detection boxes, we utilize their similarities with tracklets to recover true objects and filter out the background detections. 


* the similarity with tracklets provides a strong cue to distinguish the objects and background in low score detection boxes. 


### BYTE

* The thing is that it splits the all detection boxes into two parts: the high score boxes and the low ones

    *  Some tracklets get unmatched because they do not match to an appropriate high score detection box, which usually happens when occlusion, motion blur or size changing occurs. 
    * We then associate the low score detection boxes and these unmatched tracklets to recover the objects in low score detection boxes and filter out background, simultaneously. 

## References

-  [ByteTrack: Multi-Object Tracking by Associating Every Detection Box](https://arxiv.org/pdf/2110.06864)