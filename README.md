# Project_Lighthouse

### 🌟 The Vision
Project Lighthouse is not just an application; **it is the vision** of a world where visual impairment no longer limits spatial independence. Our goal is to engineer a seamless, intelligent "artificial eye" that restores confidence and freedom to blind individuals. By transforming raw real-time pixels into descriptive, natural conversations, Project Lighthouse allows users to truly "see" their surroundings through audio, helping them navigate, understand, and interact safely with the physical world.

---

Project Lighthouse bridges cloud-scale deep learning with immediate edge accessibility. A custom **YOLOv8** object detection and distance estimation model is trained professionally on **AWS** using thousands of open-source images, then deployed directly inside an accessible, cross-platform **Flutter** application utilizing interactive **Text-to-Speech (TTS)**.

---

## ✨ Key Features

* **Intelligent Scene Understanding:** Live camera streaming matched with customized YOLOv8 models to identify hazards, doors, personal items, and walkways.
* **Proximity & Distance Awareness:** Monocular depth estimation calculates the exact physical distance of obstacles to prevent collisions.
* **Interactive TTS Dialogue:** Integrates natural Text-to-Speech (TTS) to describe scenes smoothly and engage in helpful voice conversations with the user.
* **Accessibility-First UI/UX:** Built strictly around screen readers, high-contrast states, and large-target gesture zones for effortless non-visual navigation.

---

## 🏗️ System Architecture

The ecosystem splits across three main production environments to realize this vision:

```text
           [ 10K Images ] 
                 │
                 ▼
     [ AWS S3 / Data Pipeline ]
                 │
                 ▼
    [ AWS SageMaker (YOLOv8 GPU) ] ──► [ Quantization & TFLite/ONNX Export ]
                                                       │
                                                       ▼
                                            [ Flutter App (Client) ]
                                                       │
                                   ┌───────────────────┴───────────────────┐
                                   ▼                                       ▼
                       [ Native Edge Inference ]               [ Interactive Audio Layer ]
                       (Real-time Object & Depth)               (Flutter TTS Feedbacks)
```

---

## 🛠️ Technology Stack

* **Mobile App Framework:** Flutter (Dart)
* **Audio Layer:** Flutter TTS (`flutter_tts`) & Speech-to-Text (`speech_to_text`)
* **Object Detection Engine:** Ultralytics YOLOv8 (Optimized to TFLite / ONNX formats)
* **Mobile Inference:** Google Mobile Vision / TensorFlow Lite Flutter Interpreter
* **Cloud Infrastructure:** AWS SageMaker, Amazon S3, AWS EC2 GPU instances (`g4dn` / `g5` clusters)

---

## 📋 Repository Structure

```text
project-lighthouse/
├── flutter_app/            # Flutter mobile application client source
│   ├── assets/             # Embedded TFLite/ONNX models & audio cues
│   ├── lib/
│   │   ├── models/         # Mobile Vision inference engines & depth math
│   │   ├── services/       # TTS Audio Management & Camera Stream controllers
│   │   └── views/          # Blind-accessible UI layouts & gesture pads
│   └── pubspec.yaml        # Flutter framework dependencies
├── cloud_training/         # AWS infrastructure, datasets, and pipelines
│   ├── dataset_prep.py     # Script to pull & format open-source data (COCO/Roboflow)
│   ├── train_sagemaker.py  # SageMaker training orchestration & hyper-parameters
│   ├── export_mobile.py    # Quantization pipeline (.pt to INT8/FP16 TFLite)
│   └── requirements.txt    # Cloud-side pipeline requirements
└── README.md               # Main project documentation
```

---

## ☁️ Cloud Pipeline & Dataset Scale (AWS)

Building life-safety software for visually impaired individuals demands near-perfect model generalization across diverse settings (varying lighting, indoor/outdoor shadows, crowds).

1. **Massive Data Aggregation:** Thousands of open-source safety, navigation, and household images are aggregated, annotated in YOLO format, and synced to **Amazon S3**.
2. **SageMaker Optimization:** Heavy deep learning lifting runs via `train_sagemaker.py` utilizing scalable cloud GPUs, ensuring fast training iterations.
3. **Quantization Engine:** Following training, weights are aggressively compressed using integer quantization (`export_mobile.py`) to reduce file size and preserve battery life on standard mobile processors.

---

## 📐 Distance Estimation & Audio Logic

To tell users what is ahead, the system combines bounding-box geometry with focal lengths to compute distance dynamically. Using a known real-world physical object width ($W$), the camera's calibrated focal length ($F$), and the YOLOv8 detected pixel boundaries ($P$):

$$\text{Distance} = \frac{W \times F}{P}$$

The computed scalar is fed to the conversational TTS wrapper, converting cold metrics into intuitive verbal directions: 
> 🔊 *"Caution: Chair, 1.5 meters ahead to your left."*

---

## 🚀 Local Development Setup

### 1. Run the Flutter Mobile App
Ensure you have the Flutter SDK configured on your system.
```bash
cd flutter_app
flutter pub get
flutter run
```

### 2. Trigger Cloud Training Workflow
Set up your local AWS CLI credentials (`aws configure`) and initialize training:
```bash
cd cloud_training
pip install -r requirements.txt
python train_sagemaker.py --bucket your-s3-lighthouse-dataset-bucket --epochs 150
```

---

## 🛡️ License

This project is open-source and licensed under the MIT License.
