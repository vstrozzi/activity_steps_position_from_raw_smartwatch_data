# Mobile Health Activity Recognition 2024

## Overview

This repository contains the code and implementation for the Mobile Health (mHealth) activity recognition project completed at ETH Zurich in 2024. The project focuses on classifying human activities, sensor postions, steps and heartbeat using wearable sensor data from smartwatches placed at different body locations.

The implementation uses traditional signal processing techniques and machine learning to predict activity types (standing, walking, running, cycling), watch location (wrist, belt, ankle), walking path, and step counts from accelerometer, gyroscope, magnetometer, and altitude sensor data.

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

## Repository Structure

```
mhealth2024/
├── mhealth_activity/          # Core library for handling sensor recordings
│   ├── __init__.py           # Package initialization
│   ├── recording.py          # Recording class for managing sensor data
│   ├── trace.py              # Trace class for individual sensor streams
│   └── types.py              # Enumerations (Activity, WatchLocation, Path)
├── submission_code/          # Competition submission code
│   └── submission_template.ipynb  # Main notebook with full pipeline
├── packagesCPU. txt           # Python package dependencies
└── README.md                 # This file
```

## Key Components

### Data Processing Pipeline

The `mhealth_activity` module provides classes for handling sensor data:

- **`Recording`**: Manages complete sensor recordings with multiple data traces
  - Loads data from pickle files
  - Supports plotting and visualization
  - Handles metadata (labels, activity types, watch location)

- **`Trace`**: Represents individual sensor streams
  - Stores time-series sensor values
  - Manages timestamps and sample rates
  - Provides interpolation capabilities

- **`Types`**: Defines enumerations for: 
  - Activities:  STANDING, WALKING, RUNNING, CYCLING
  - Watch Locations: WRIST, BELT, ANKLE
  - Paths: PATH_0 through PATH_4

### Feature Engineering

The implementation extracts comprehensive features from sensor data:

**Statistical Features** (per window):
- Mean, standard deviation
- 25th and 75th percentiles
- Energy and maximum frequency

**Sensor Modalities**:
- Accelerometer (ax, ay, az)
- Gyroscope (gx, gy, gz)
- Magnetometer (mx, my, mz)
- Altitude sensor

**Temporal Features**:
- Step count using peak detection algorithm
- Window-based aggregations
- Cross-sensor correlations

### Step Detection Algorithm

A bandpass filter-based peak detection algorithm identifies steps from accelerometer magnitude: 
- Bandpass filtering (1-3 Hz) to isolate walking frequencies
- Adaptive peak detection with configurable thresholds
- Distance-based peak separation to avoid double-counting

### Classification Models

Three separate XGBoost classifiers are trained:

1. **Activity Classifier**: Predicts standing, walking, running, cycling
2. **Watch Location Classifier**: Predicts wrist, belt, or ankle placement
3. **Path Classifier**: Predicts walking path index (0-4)

Each model uses task-specific feature selection to optimize performance: 
- Activity classification excludes low-frequency sensors (altitude, magnetometer)
- Watch location uses all accelerometer and gyroscope features
- Path classification focuses on altitude and magnetometer data
