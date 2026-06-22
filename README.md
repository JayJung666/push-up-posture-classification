# Push-up Posture Classification Dataset

## Overview
This dataset contains joint angle feature sequences extracted from push-up
repetition videos for posture quality classification. It accompanies the paper:

> "Classification of Push-up Posture Quality Using Lightweight LSTM
> Architecture and MediaPipe-based Joint Angle Extraction"
> Antonius Vincent Jung, Bedy Purnama, and Bayu Erfianto
> ICoDSA 2026 (to appear)

---

## Dataset Description

| Property | Value |
|----------|-------|
| Total samples | 315 repetitions |
| Subjects | 8 participants |
| Classes | 2 (correct: 172, incorrect: 143) |
| Error types (incorrect) | Hip sagging, hip pike, incomplete range of motion |
| Recording view | Sagittal |
| Recording specs | 30 FPS, Full HD (1920×1080), Android smartphone |
| Annotation | Validated by a certified fitness trainer |

---

## Feature Extraction Pipeline

Keypoints were extracted using **MediaPipe PoseLandmarker** (33 landmarks).
Six landmarks from the right side were used: shoulder, elbow, wrist, hip,
knee, and ankle. Four joint angles were computed via the **arctan2** function:

| Feature | Proximal | Joint | Distal |
|---------|----------|-------|--------|
| Elbow angle | Shoulder | Elbow | Wrist |
| Shoulder angle | Elbow | Shoulder | Hip |
| Hip angle | Shoulder | Hip | Knee |
| Knee angle | Hip | Knee | Ankle |

> **Note:** Raw sequences in this dataset are stored at their **original frame
> count** (ranging from 36 to 144 frames per repetition). Temporal resampling
> to **T_fixed = 60 frames** is performed during data loading in the training
> pipeline using uniform subsampling (for sequences longer than 60 frames)
> and linear interpolation (for sequences shorter than 60 frames).

---

## Repository Structure

```
push-up-posture-classification/
│
├── README.md
├── LICENSE
└── data/
    ├── data_features/
    │   ├── S01_1_001.csv
    │   ├── S01_1_002.csv
    │   └── ...
    └── metadata_pushup_final.csv
```

---

## File Format

### data/data_features/
Each `.csv` file represents **one full repetition cycle**.

**Filename format:** `{SubjectID}_{SetNumber}_{RepetitionNumber}.csv`

| Part | Example | Description |
|------|---------|-------------|
| SubjectID | S01 | Subject identifier (S01–S08) |
| SetNumber | 1 | Recording set number |
| RepetitionNumber | 001 | Repetition index within the set |

**Columns (17 total):**

| Column | Description |
|--------|-------------|
| `frame` | Frame index (1-based) |
| `shoulder_x`, `shoulder_y` | Shoulder landmark (normalized coordinates) |
| `elbow_x`, `elbow_y` | Elbow landmark (normalized coordinates) |
| `wrist_x`, `wrist_y` | Wrist landmark (normalized coordinates) |
| `hip_x`, `hip_y` | Hip landmark (normalized coordinates) |
| `knee_x`, `knee_y` | Knee landmark (normalized coordinates) |
| `ankle_x`, `ankle_y` | Ankle landmark (normalized coordinates) |
| `elbow_angle` | Elbow joint angle in degrees (via arctan2) |
| `shoulder_angle` | Shoulder joint angle in degrees (via arctan2) |
| `hip_angle` | Hip joint angle in degrees (via arctan2) |
| `knee_angle` | Knee joint angle in degrees (via arctan2) |

### data/metadata_pushup_final.csv
Metadata for all 315 samples including subject ID, label, and error type.

---

## Evaluation Scheme

Models were evaluated using **Group 4-Fold Cross-Validation**:

| Fold | Train | Validation | Test |
|------|-------|------------|------|
| 1 | S04, S08, S03, S06 | S01, S05 | S02, S07 |
| 2 | S02, S07, S03, S06 | S04, S08 | S01, S05 |
| 3 | S02, S07, S01, S05 | S03, S06 | S04, S08 |
| 4 | S02, S07, S01, S05 | S04, S08 | S03, S06 |

No subject appears in both training and test set, ensuring leakage-free
evaluation.

---

## Citation

If you use this dataset, please cite:

```bibtex
@inproceedings{jung2026pushup,
  author    = {Jung, Antonius Vincent and Purnama, Bedy and Erfianto, Bayu},
  title     = {Classification of Push-up Posture Quality Using Lightweight
               LSTM Architecture and MediaPipe-based Joint Angle Extraction},
  booktitle = {Proceedings of ICoDSA 2026},
  year      = {2026}
}
```

---

## License

This dataset is licensed under
[Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).
You are free to use, share, and adapt this dataset as long as
appropriate credit is given.

---

## Contact

This dataset was collected and curated by Antonius Vincent Jung.
For questions regarding this dataset or to request access to the
raw video recordings, please contact:

**Antonius Vincent Jung**
vincentjung@student.telkomuniversity.ac.id
antoniusvincent09@gmail.com
