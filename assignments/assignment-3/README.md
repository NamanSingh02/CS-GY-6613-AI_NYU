# Assignment 3 — UAV Drone Detection and Tracking

**Student:** Naman Singh  
**Course:** AI Spring 2026 
**Assignment:** Assignment 3
**Submission date:** 22nd March 2026
 
**Hugging Face Commit ID:** 67e63888f1b573ee13874c6ce85b33d42b7af4aa
https://huggingface.co/datasets/Naman02/assignment3-drone-detections2

**Youtube tracked video links:**

https://youtu.be/jyfp96JDxZg

https://youtu.be/5BUGWK-5HGU

## Overview

This project implements an end-to-end **drone detection and tracking** pipeline on assignment test videos.

The system:
- runs a **deep learning drone detector** on video frames,
- saves sample frames with detections to a **Hugging Face dataset** in **Parquet** format,
- tracks detected drones over time using a **Kalman filter**,
- and generates **annotated output videos** with bounding boxes and trajectories.



## Dataset choice and detector configuration

### Dataset choice

I used:

- **Hugging Face dataset:** `pathikg/drone-detection-dataset`  
  This dataset contains labeled drone bounding boxes and is suitable for training / fine-tuning a detector for UAV/drone detection.

### Detector choice
I used a **deep learning object detector** based on **Ultralytics YOLO**.

Workflow:
- Start from a pretrained drone detector / YOLO model.
- Fine-tune it on the drone bounding-box dataset for improved accuracy.
- Use the fine-tuned model for inference on the assignment videos.

### Detector configuration
- Model family: **YOLO**
- Task: **single-class drone detection**
- Input size: higher input resolution was used to improve small-object detection
- Fine-tuning: lightweight fine-tuning was performed to improve performance while keeping Colab runtime manageable
- Inference threshold: confidence filtering was applied to suppress weak detections

### Why YOLO was used
YOLO was chosen because:
- it is easy to fine-tune in Colab,
- it provides strong speed/accuracy tradeoffs,
- and it integrates cleanly into a full video + tracking pipeline.



## Kalman filter state design and noise parameters

### State design
Tracking is performed using a **Kalman filter** with a **constant-velocity motion model**.

The tracker models the drone using its image-plane motion. Conceptually, the state includes:
- the object position in the frame,
- and its velocity over time.

A typical representation is:
- position: object center `(cx, cy)`
- velocity: `(vx, vy)`

The detector provides measurements from the bounding box each frame, and the Kalman filter smooths these measurements over time.

### Prediction and update
For each frame:
1. The Kalman filter **predicts** where the drone should appear next.
2. If a detection is available, the tracker **updates** the state using the new measurement.
3. If a detection is missing, the tracker continues with the **predicted state** for a limited number of frames.

### Noise parameters
The filter uses:
- **process noise** to allow for motion uncertainty,
- **measurement noise** to account for noisy detector outputs.

These noise parameters were chosen to balance:
- smooth trajectories,
- resistance to jitter,
- and recovery from short detection gaps.

In practice:
- **process noise** is set high enough to let the tracker follow motion changes,
- **measurement noise** is set to trust detections while still smoothing unstable boxes.

The exact values are defined in the notebook implementation and were tuned empirically for stable tracking on the assignment videos.

## Failure cases and how the tracker handles missed detections

### Failure cases
The system can fail or degrade in the following situations:

1. **Small or distant drones**  
   The drone occupies very few pixels, making detection difficult.

2. **Fast motion / motion blur**  
   Rapid movement can reduce detector confidence and cause short missed detections.

3. **Occlusion or partial visibility**  
   Trees, buildings, clouds, or other objects can partially hide the drone.

4. **Background confusion**  
   The detector may confuse the drone with visually similar small objects or background clutter.

5. **Lighting / contrast changes**  
   Bright sky, low contrast, or poor lighting can reduce detection quality.

### Missed-detection handling
The tracker is designed to handle short detection failures gracefully.

When a frame has no valid detection:
- the Kalman filter still performs a **prediction step**,
- the object is propagated forward using the estimated motion,
- and the trajectory remains continuous for short gaps.

A track is only terminated after missing detections for too many consecutive frames.  
This avoids immediate track loss from a single bad frame.

### Why this helps
This design improves robustness because:
- short detector dropouts do not immediately break the trajectory,
- the motion estimate provides temporal continuity,
- and the final output video remains smoother and easier to interpret.



## Hugging Face dataset

The Hugging Face dataset contains the **sample frames with detections** in **Parquet format**, as required.

Dataset link:  
**`https://huggingface.co/datasets/Naman02/assignment3-drone-detections2`**




## Output tracking videos

I generated one output tracking video per test input.  
These videos contain:
- detection bounding boxes,
- tracked drone position,
- and trajectory visualization.

### Video 1
YouTube link: `https://youtu.be/jyfp96JDxZg`

### Video 2
YouTube link: `https://youtu.be/5BUGWK-5HGU`
