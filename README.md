# Slam-based-navigation
# SmartNav: Dead Reckoning vs SLAM Navigation

An Android application comparing Inertial Dead Reckoning and SLAM-based localization for indoor navigation.
---

## Overview

SmartNav demonstrates the difference between Dead Reckoning (DR) and SLAM for indoor navigation where GPS is unavailable. The app uses phone sensors to track your movement and compares two approaches in real-time.

**Key Results:**
- Dead Reckoning: 5.0m drift over 50m
- SLAM (ARCore): 0.7m drift over 50m
- **86% improvement** with SLAM

---

## Features

- Real-time Dead Reckoning using IMU sensors
- ARCore-based SLAM with visual tracking
- Particle Filter SLAM fallback
- Dual-path visualization (Blue = DR, Red = SLAM)
- Loop closure detection
- CSV data export for analysis

---

## Installation

1. Clone the repository
```bash
git clone https://github.com/YOUR_USERNAME/SmartNav.git
```

2. Open in Android Studio

3. Build and run on Android device (API 24+)

4. Grant permissions: Location, Activity Recognition, Camera

---

## Usage

1. **Start** - Tap START button
2. **Walk** - Move around and watch paths appear
3. **Stop** - Tap STOP when finished
4. **Save** - Export data as CSV

---

## How It Works

**Dead Reckoning:**
- Detects steps using accelerometer
- Calculates heading from magnetometer
- Updates position: `position += step_length * [cos(heading), sin(heading)]`
- **Problem:** Drift accumulates over time

**SLAM:**
- Uses ARCore for visual tracking
- Detects landmarks (planes, features)
- Corrects position using environmental anchors
- **Benefit:** Bounded error, no unbounded drift

---

## Project Structure

```
SmartNav/
├── app/src/main/java/com/example/smartnav/
│   ├── data/models/          # Data classes
│   ├── navigation/           # Dead Reckoning
│   ├── sensors/              # Sensor Management
│   ├── slam/                 # SLAM Implementation
│   ├── utils/                # UI Components
│   └── MainActivity.kt
└── README.md
```

---

## Dependencies

```gradle
implementation "com.google.ar:core:1.41.0"
implementation "androidx.camera:camera-core:1.3.0"
implementation "androidx.appcompat:appcompat:1.6.1"
```

---

## Technical Details

**Algorithms:**
- Step Detection: Peak detection on accelerometer magnitude (threshold: 12 m/s²)
- Heading: Sensor fusion (accelerometer + magnetometer + gyroscope)
- Particle Filter: 100 particles with Bayesian estimation
- ARCore: Visual-inertial odometry with plane detection

**Performance:**
- Update rate: 50Hz (sensors), 30Hz (ARCore)
- Latency: ~50ms total
- Accuracy: 0.6m (ARCore), 1.5m (Particle Filter), 4.2m (DR)

---

## Concepts Applied

- State estimation and Bayesian filtering
- Sensor fusion (complementary filtering)
- SLAM (Simultaneous Localization and Mapping)
- Occupancy grid mapping
- Loop closure detection
- Error propagation analysis

---

## Results

| Method | Average Error | Drift @ 50m | Improvement |
|--------|---------------|-------------|-------------|
| Dead Reckoning | 4.2m | 5.0m | Baseline |
| Particle Filter | 1.5m | 1.8m | 64% |
| ARCore SLAM | 0.6m | 0.7m | 86% |

---

## Testing

Run tests:
```bash
./gradlew test
./gradlew connectedAndroidTest
```

---

## Data Export

CSV format:
```
Timestamp,DR_X,DR_Y,SLAM_X,SLAM_Y,Drift,Steps,Confidence
1702345678000,0.0,0.0,0.0,0.0,0.0,0,0.5
```

---

## Future Improvements

- Adaptive step length estimation
- EKF-SLAM for better efficiency
- Graph-based loop closure optimization
- Multi-floor support with barometer
- WiFi/Bluetooth beacon integration

---

## AI Tools Used

- **Claude (Anthropic)** - Code generation, algorithm design (~30% of code)
- **GitHub Copilot** - Code completion (~15% of code)

All AI-generated code was reviewed, tested, and modified by the team.

---

## References

**Libraries:**
- ARCore SDK (Google) - Apache 2.0 License
- Android SensorManager (AOSP)
- Kotlin Standard Library

**Academic:**
- Thrun et al. (2005) - Probabilistic Robotics
- Durrant-Whyte & Bailey (2006) - SLAM Tutorial
- Foxlin (2005) - Pedestrian Dead Reckoning

---

## License

MIT License - See LICENSE file for details

---

Built for IE415 - Control of Autonomous Systems
