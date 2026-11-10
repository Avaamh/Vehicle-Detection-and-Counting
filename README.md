# Vehicle detection and counting

A traffic-video analysis project developed for the **Image Processing** course during my bachelor's studies.

**Author:** Ava Moshfegh  
**Completed:** 31 December 2024

Since this project was done on Google Colab, I no longer have access to my codes, therefore I only describe what I had done.

The project detects and counts **cars, buses, and trucks** in traffic videos. It combines image preprocessing, deep-learning detection, vehicle tracking, and directional counting to study how different processing choices affect the final counts.

The work focuses on comparing preprocessing and detection configurations, checking predicted counts against annotated video data, and evaluating performance across different traffic scenes, including nighttime footage.

## Features

- Detection and classification of cars, buses, and trucks.
- Frame preprocessing and comparison with unprocessed input.
- Deep-learning vehicle detection.
- Tracking across frames to maintain vehicle identities.
- Directional counting with separate incoming and outgoing totals.
- Counts reported separately for each vehicle class.
- Video annotation for evaluation.
- Experiments on multiple traffic videos and additional standard datasets.
- Comparison of preprocessing configurations and counting results.

## Vehicle classes

| Class | Description |
| --- | --- |
| Car | Passenger vehicles. |
| Bus | Buses appearing in the traffic scene. |
| Truck | Trucks appearing in the traffic scene. |

Keeping counts separate by class makes it possible to examine errors that would be hidden in a single combined vehicle total.

## Processing workflow

The system processes traffic footage through five stages: video input, preprocessing, detection, tracking, and counting.

### 1. Video input

Frames are read from each traffic video in sequence. Each video is evaluated as a separate scene so that its camera view, traffic direction, and lighting conditions can be considered when configuring the counting regions.

The video collection includes a nighttime sequence for examining performance under reduced visibility.

### 2. Image preprocessing

Preprocessing prepares frames for vehicle detection. The processing workflow includes contrast enhancement and filtering, with configurations compared against the original input.

| Operation | Purpose |
| --- | --- |
| CLAHE | Adjusts local contrast to improve visibility in image regions with uneven illumination. |
| Gaussian filtering | Smooths image noise before detection. |
| Preprocessing comparison | Examines whether a transformation improves or worsens the final vehicle counts. |

Preprocessing is evaluated through the downstream detection and counting results. A visually clearer frame does not automatically produce a more accurate count, so comparisons include both processed and unprocessed video.

### 3. Vehicle detection

Deep-learning object detection identifies vehicles and assigns them to the car, bus, or truck class. The detection stage produces the locations and class information needed by the tracker and counter.

The model-comparison workflow considers YOLOv8 and YOLOv9 configurations. Model choice is evaluated using the resulting vehicle counts rather than assuming that a newer detector will always perform better.

### 4. Tracking

Tracking connects detections across consecutive frames. The workflow uses ByteTrack to associate observations with vehicle identities, allowing the counter to follow a vehicle's movement through the scene.

This separates the number of vehicles from the number of frame-level detections: a vehicle visible in many frames should not be counted again simply because it has been detected again.

### 5. Directional counting

A multi-zone counting approach uses vehicle trajectories and configured counting lines or regions to distinguish incoming and outgoing traffic.

For each video, the system maintains separate totals for:

| Direction | Counts |
| --- | --- |
| Count-in | Cars, buses, and trucks moving in the incoming direction. |
| Count-out | Cars, buses, and trucks moving in the outgoing direction. |

Counting-region placement depends on the scene. Lines must correspond to the traffic flow visible in that video so that crossing events represent the intended direction of travel.

## Evaluation

Evaluation compares predicted counts with annotated reference counts. Results are examined by video, vehicle class, and direction rather than only as a combined total.

The experimental comparisons cover:

- Original input versus preprocessed input.
- Different preprocessing configurations.
- Different detector configurations.
- Incoming versus outgoing traffic counts.
- Car, bus, and truck counts separately.
- Different video scenes, including nighttime footage.
- Additional standard datasets to examine performance beyond the initial videos.

The result tables record the video, model configuration, preprocessing configuration, vehicle class, direction, reference count, and predicted count. This makes it possible to identify undercounting and overcounting for each configuration and explain the effect of a change.

Video labeling provides the reference information for these comparisons. The analysis also examines changes to the counting approach as part of improving the complete detection-to-counting workflow.

## Traffic videos

The primary video set contains five files:

| Video | Download |
| --- | --- |
| `TestVideo1_GU.mp4` | [Google Drive](https://drive.google.com/file/d/1E3x067gH_9_3PnPoJ4xKTj3FOHVpksRN/view?usp=drive_link) |
| `TestVideo2.mp4` | [Google Drive](https://drive.google.com/file/d/1mqC-VvOosD7ndxo5y4xZlmZ3zxSeHACN/view?usp=drive_link) |
| `TestVideo3.mp4` | [Google Drive](https://drive.google.com/file/d/1iS80k4STitrbLJInNc7SBs53rWKh-EoW/view?usp=drive_link) |
| `TestVideo4.mp4` | [Google Drive](https://drive.google.com/file/d/1sQx_KPJ5KN9OLNm6vCmJNKLAvbAt5GE7/view?usp=drive_link) |
| `TestVideo5_Night.mp4` | [Google Drive](https://drive.google.com/file/d/1Tfc7E7sUoCLosasijwsBR2C0tll-DGHR/view?usp=drive_link) |

## Experiment procedure

1. Select a traffic video and its corresponding annotations or reference counts.
2. Configure the incoming and outgoing counting regions for the camera view.
3. Run the detection, tracking, and counting workflow on the original video.
4. Repeat with the selected preprocessing configurations.
5. Compare detector configurations using the same video and counting setup.
6. Record the car, bus, and truck totals for both directions.
7. Compare predictions with the reference counts and inspect examples of missed or repeated counts.
8. Repeat the evaluation across the remaining videos and additional datasets.

Keep the video, class definitions, reference counts, and counting regions consistent when comparing preprocessing or model configurations. This helps distinguish the effect of the tested change from differences in the evaluation setup.

## Project focus

The project examines vehicle counting as a complete video-processing task. Detection quality, identity tracking, preprocessing, and counting-region placement all affect the reported totals. The experimental work compares these choices using class-specific and direction-specific counts, with particular attention to differences between traffic scenes and lighting conditions.
