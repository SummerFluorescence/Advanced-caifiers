# Advanced-caifiers
## Task

We classify dermoscopic images into seven diagnostic categories.
Our goal is to use visualizations to understand model behavior,
including common confusions, high-confidence errors, and differences
between correctly classified and misclassified images.

## Model

We use an ImageNet-pretrained ResNet-50, fine-tuned on the seven
HAM10000 diagnostic classes.

A checkpoint is saved after each training epoch. The best checkpoint
is selected using validation balanced accuracy.

Both pairs use the same trained model and data split.

## Dataset

We use HAM10000, which contains 10,015 dermoscopic images
from seven diagnostic categories:

- akiec: Actinic keratoses and intraepithelial carcinoma
- bcc: Basal cell carcinoma
- bkl: Benign keratosis-like lesions
- df: Dermatofibroma
- mel: Melanoma
- nv: Melanocytic nevi
- vasc: Vascular lesions

The dataset includes images and metadata. Lesion segmentation masks
are downloaded separately for image analysis.

The data are split into 70% training, 15% validation, and 15% test
sets at the lesion level. Images belonging to the same lesion
remain in the same split.

The class distribution is imbalanced, with nv being the largest class.

## Shared Outputs

The training notebook produces the following files:

- `splits.csv`: Image IDs, lesion IDs, and split assignments.
- `checkpoints/epoch_XX.pt`: Model checkpoints for each epoch.
- `checkpoints/best.pt`: Best model by validation balanced accuracy.
- `train_history.csv`: Training and validation metrics.
- `predictions_all.csv`: Metadata, predictions, class probabilities,
  confidence, entropy, malignancy probability, and lesion area.
- `embeddings_all.npy`: 2048-dimensional features for Phase II only.

The starting visualization is a confusion matrix linked to an
image thumbnail grid.
