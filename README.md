# Global Wheat Head Detection

Détection automatique des épis de blé (*wheat heads*) à partir d'images de champs, avec une évaluation de la capacité de généralisation inter-pays (Leave-One-Domain-Out) sur le dataset Global Wheat Head Detection (GWHD).

## Contexte

Le comptage et la localisation des épis de blé permettent d'estimer la densité et la maturité des cultures — une information clé pour le phénotypage agronomique. Ce projet compare deux architectures de détection d'objets (YOLOv8 et Faster R-CNN avec backbone Swin Transformer) et évalue leur robustesse face au *domain shift* entre pays.

## Structure du repo
├── data/ # Données du dataset GWHD (images + annotations train.csv)
├── notebooks/
│ ├── EDA/ # Analyse exploratoire : distribution, luminance,
│ │ # distance MMD (ResNet50) entre pays
│ ├── FasterRCNN/ # Entraînement et évaluation Faster R-CNN + Swin-T
│ │ # (MMDetection) en configuration LODO
│ └── Yolo8/ # Entraînement et évaluation YOLOv8 (Ultralytics)
│ # en configuration LODO


## Méthodologie

1. **EDA** — caractérisation du décalage de domaine (*domain shift*) entre les 7 sources du dataset via une distance MMD calculée sur des features ResNet50
2. **Protocole d'évaluation** — Leave-One-Domain-Out (LODO) : chaque pays sert tour à tour de domaine de test, exclu de l'entraînement, pour mesurer la vraie capacité de généralisation
3. **Modèles comparés**
   - **YOLOv8** (Ultralytics) — détecteur one-stage
   - **Faster R-CNN + Swin Transformer** (MMDetection) — détecteur two-stage
4. **Métrique** — mAP@50

## Résultats principaux

| Modèle | mAP@50 (moyenne LODO) |
|---|---|
| YOLOv8 | 0,83 – 0,96 |
| Faster R-CNN + Swin-T | 0,54 – 0,78 |

## Dataset

[Global Wheat Head Detection Dataset](https://www.kaggle.com/competitions/global-wheat-detection)

## Technologies

- Python, TensorFlow/Keras (extraction features ResNet50)
- Ultralytics (YOLOv8)
- MMDetection / MMCV (Faster R-CNN + Swin Transformer)
- scikit-learn (calcul MMD)
