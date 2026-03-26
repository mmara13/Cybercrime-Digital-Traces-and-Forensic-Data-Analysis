# Detecting Stabbing Motions Using Smartwatch Sensor Data
### Cybercrime Digital Traces and Forensic Data Analysis - University of Amsterdam, 2026
**Felianne van Leersum · Mara-Andreea Spataru · Lotte Summerfield-Son**

---

## About this project

This repository contains all data, annotation files, and code for our project. We collected smartwatch sensor data from three participants performing five activities -- stabbing, vacuuming, brushing teeth, brushing hair, and doing nothing -- and trained three machine learning models to distinguish the stabbing motion from the everyday activities.

The full paper is available [ADD PDF].

---

## Repository structure

```
/
├── Faye_Annotation_file.csv        # manually logged activity timestamps for Participant 1
├── Lotte_Annotation_file.csv       # manually logged activity timestamps for Participant 2 (Lotte)
├── Mara_Annotation_file.csv        # manually logged activity timestamps for Participant 2 (Mara)
├── faye stabbing/                  # raw sensor files - Faye, stabbing session
├── faye 1/                         # raw sensor files - Faye, session 1
├── lotte/                          # raw sensor files - Lotte
├── mara 1/                         # raw sensor files - Mara, session 1
└── mara 2/                         # raw sensor files - Mara, session 2
```

Each sensor folder contains five CSV files, one per sensor stream:
`WatchAccelerometer_*.csv`, `WatchGyroscope_*.csv`, `WatchGravity_*.csv`,
`WatchOrientation_*.csv`, `WatchTotalAcceleration_*.csv`

---

## How to run the code

The code runs in **[Google Colab](https://colab.research.google.com/drive/1HMlcbo2Wpr7TsZtoWocHP3g9fOjT9ARL?usp=sharing#scrollTo=wzKAUCogJkDG)**. 

### Step 1 - Download the data

Download the following files from this repository:
- The three annotation files (`Faye_Annotation_file.csv`, `Lotte_Annotation_file.csv`, `Mara_Annotation_file.csv`)
- All five sensor folders (`faye stabbing`, `faye 1`, `lotte`, `mara 1`, `mara 2`)

### Step 2 - Upload to Google Colab

Open Google Colab and upload all downloaded files to the session storage.

### Step 3 - Run the pipeline

The code is split into two multiple code cells:

1. **preprocessing of data** - preprocesses the data and defines all helper functions needed

2. **session** - defines all sessions for all participants

3. **first pipeline** - loads the raw sensor files, merges sensor streams, applies annotations, simplifies labels, and saves three output CSV files: `faye_final.csv`, `maralotte_final.csv`, and `all_participants_final.csv`

4. **sparse-row removal**

5. **last pipeline** - loads `all_participants_final.csv`, downsamples to 30 Hz, extracts sliding window features, trains the three models, and evaluates them

### Step 4 - Outputs

All results are printed directly in the Colab output. Additionally, the following files are saved to the local Colab runtime:

- `model_results_summary.csv` - summary table of all model results
- `cm_RandomForest_random_80_20_normalized.png` - confusion matrix, random split
- `cm_RandomForest_lopo_test_faye_normalized.png` - confusion matrix, LOPO test P1
- `cm_RandomForest_lopo_test_maralotte_normalized.png` - confusion matrix, LOPO test P2
- `feature_importance_rf_random.png` - feature importance, random split
- `feature_importance_rf_lopo_faye.png` - feature importance, LOPO test P1
- (and confusion matrices for all 9 model–split combinations)

*Note that all confusion matrices have been normalized.*
---

## Dependencies

All required packages are available in Google Colab by default, except CatBoost. The ML pipeline installs it automatically with:
```python
!pip install catboost -q
```
No other setup is needed.

---

## Data collection notes

- Device: Samsung Galaxy Watch 7
- App: SensorLogger (version 1.52.0)
- Participants: 3 (all researchers, right-handed)
- Activities: stabbing, vacuuming, brushing teeth, brushing hair, doing nothing
- Each activity was recorded for 2 minutes per session
- Recordings were affected by intermittent gaps due to Bluetooth connection loss between the watch and the paired phone (confirmed by the SensorLogger developer). Sessions with insufficient recoverable data were excluded.

---
