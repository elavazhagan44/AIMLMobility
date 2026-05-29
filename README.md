# AIMLMobility
AI and ML for Mobility Applications assignment

# Recording video link:
https://drive.google.com/file/d/1LMKgLZlWLL84oQZz7T_VEykjjvEy6ROR/view?usp=drivesdk

# Project Executive Summary & Objective
In automotive engineering, testing systems rely heavily on the Verification and Validation (V&V) Cycle to ensure data integrity and system reliability. This project applies a rigorous V&V workflow to a continuous screen recording of a real-world vehicle journey.

The primary objective is to process a vehicle journey spanning 21.5 minutes with an actual mapped ground-truth distance of 15.0 km. The project analyzes data reliability by contrasting a manual, human-observed baseline (Verification Phase) against an advanced automated computer vision and character recognition pipeline (Validation Phase).

# Phase A: Manual Verification Workflow (Sub-task 4A)
Data Sampling Strategy: Telemetry data was manually extracted by observing the journey recording at strict 30-second intervals, compiling a foundational baseline table of 45 continuous velocity checkpoints.

Graph Sheet Scaling Parameters: The extracted coordinates were plotted on a physical grid paper using custom scaling factors: 
X-Axis (Time): 1 cm = 1 minute = 1/60 hour 
Y-Axis (Velocity): 1 cm = 5 km/h
Mathematical Resolution Per Unit Grid Box: Area of 1 Box = (1/60 hr) * 5 km/h = 0.0833 km per box

Manual Counting Results: 
A precise human box-counting calculation tracking individual fractional spaces underneath the velocity line yielded a total sum of exactly 145 boxes. 
Total Manual Distance = 145 boxes * 0.0833 km/box = 12.083 km

# Phase B: Automated Computer Vision Pipeline (Sub-task 4B & 5)
To engineer a scalable validation mechanism, a modular automated pipeline was constructed using Python, OpenCV, and EasyOCR. This system directly reflects the processing stages of the Machine Learning Basics lifecycle:
A. Data Preprocessing & Target Cropping: The script processes the raw video stream at programmatically defined time windows (gap_seconds = 30). To filter out external dashboard noise, cockpit shadows, or windshield glare, a responsive percentage-based boundary mask crops the frame down to the targeted speedometer Region of Interest (ROI):

crop = frame[int(h0.77):int(h0.87), int(w0.06):int(w0.22)]

B. Image Masking & Optical Character Recognition (OCR): The cropped vehicle dashboard matrix is isolated into a single-channel grayscale image and passed through a high-pass binary threshold filter (cv2.threshold targeting an intensity value of 225). This eliminates background variance and sharpens the edges of the text font, allowing the EasyOCR neural network engine to read the pixel matrices and output clean digital telemetry digits.

C. Contextual Filtering & Data Integrity Logic: To prevent tracking spikes, occlusions, or numeric characters from breaking data consistency (such as interpreting video artifacts as a speed value of 4, 6, or 65 what time vehicle is idling traffic), intelligent boundary rules evaluate each step against the last_valid_speed record.

D. Programmatic Numerical Integration: Instead of manual box grouping, the cumulative trip mileage is computed automatically at each step using a numeric integration method matching the time intervals:
added_dist = current_speed * (gap_seconds / 3600)

# Automation Pipeline Flowchart Diagram

4. Automation Pipeline Flowchart Diagram
<img width="703" height="590" alt="image" src="https://github.com/user-attachments/assets/baeac6f9-ed6a-4006-8893-b4835187aeb2" />



# Model Evaluation, Analytics & Error Calculations (Sub-task 6)
The automated Python architecture processed the timeline up to 1290 seconds, tracking the vehicle's dynamic acceleration profiles and exporting the final data deliverables.

Mobility Analytics Metrics Summary Table:
Actual Reference Map Distance: 15.000 km
Automated Integrated Distance: 13.433 km
Computed Final Model Error: 10.44%

Telemetry Visualizations: The software pipeline auto-generates a dual-panel visualization dashboard (automated_journey_plots.png) used for performance analysis:

Velocity Profile Chart (Left Panel): Tracks speed over time, explicitly mapping the vehicle's real-world behavior, transient acceleration phases, highway cruise thresholds peaking at 83 km/h, and clear zero-velocity idling periods (signal stops).

Distance Covered Curve (Right Panel): Tracks the continuous monotonic accumulation of the computed trip mileage across the duration of the video against baseline limits.

<img width="1671" height="683" alt="image" src="https://github.com/user-attachments/assets/fbb68dd2-2f05-46b0-9baa-6069ca49df28" />


