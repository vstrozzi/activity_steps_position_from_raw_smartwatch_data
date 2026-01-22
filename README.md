# Mobile Health Activity Recognition 2024

![Teaser](sumbission_code/ppg_orig.png)
![Teaser](sumbission_code/ppg_filt.png)

## Overview

This repository contains the code and implementation for the Mobile Health (mHealth) activity recognition project completed at ETH Zurich in 2024. The project focuses on classifying human activities, sensor postions, steps and heartbeat using wearable sensor data from smartwatch (LilyGo) placed at different body locations.

The implementation uses traditional signal processing techniques and machine learning to predict activity types (standing, walking, running, cycling), watch location (wrist, belt, ankle), walking path, and step counts from accelerometer, gyroscope, magnetometer, and altitude sensor data (sensor fusion)

By Virgilio Strozzi at ETH Zürich (2024).
## Abstract

Wearable sensors have become increasingly prevalent for health monitoring and activity tracking. This project investigates the classification of human activities and step detection using sensor data collected from smartwatches positioned at different body locations. The system processes multi-modal sensor data including accelerometer, gyroscope, magnetometer, and altitude measurements to accurately identify user activities and movement patterns.

The implementation employs feature engineering on windowed sensor data, extracting statistical features (mean, standard deviation, quantiles) and frequency-domain characteristics.  Multiple XGBoost classifiers are trained to predict: 
- **Activity Type**: Standing, walking, running, or cycling
- **Watch Location**:  Wrist, belt, or ankle placement
- **Path Index**: Different walking paths (0-4)
- **Step Count**: Number of steps taken during the activity
- (**Heart Rate**: from previous project)

The models are trained on segmented time-series data with a 60-second window length and 50% overlap, achieving robust classification performance across different sensor configurations and body placements.
