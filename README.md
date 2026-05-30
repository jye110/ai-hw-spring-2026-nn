# MNIST Neural Network Classification

This repository contains my Artificial Intelligence homework for building and testing small image recognition neural networks on the MNIST handwritten digit dataset.

## Assignment

**Problem:** Make a small image recognition neural network.

**Dataset:** MNIST handwritten digit dataset.

The model is trained only on the official MNIST training set and evaluated only on the official MNIST test set.

The assignment asks us to experiment with the following neural network models:

- Shallow Multi-Layer Perceptron (MLP)
- Convolutional Neural Network (CNN)
- Transformer Encoder

I also tested different image augmentation settings and compared the final test accuracy.

## Repository Structure

```text
ai-hw-spring-2026-nn/
├── MNIST.ipynb        # Main notebook: MLP, CNN, Transformer, augmentations, training, and testing
├── CNN_MNIST.ipynb    # Additional CNN experiment notebook
└── README.md
```

## Dataset

The project uses the MNIST dataset from `torchvision.datasets.MNIST`.

MNIST contains grayscale handwritten digit images:

- Image size: 28 × 28
- Number of classes: 10 digits, from 0 to 9
- Training set: 60,000 images
- Test set: 10,000 images

In this project:

- `train=True` is used for training.
- `train=False` is used for final testing.
- The test set is not used during training.

## Models

### 1. Shallow MLP

The MLP is a simple fully connected neural network.
203,530 parameters

Architecture:

```text
Input image: 1 × 28 × 28
Flatten: 784 features
Linear layer: 784 → 256
ReLU activation
Dropout
Linear layer: 256 → 10
Output: digit class logits
```

The MLP treats the image as a flat vector, so it does not directly preserve spatial information.

### 2. CNN

The CNN uses convolutional layers to learn local spatial patterns from the image.
421,642 parameters

Architecture:

```text
Input image: 1 × 28 × 28
Conv2D: 1 → 32
ReLU
MaxPool: 28 → 14
Conv2D: 32 → 64
ReLU
MaxPool: 14 → 7
Flatten
Linear layer: 64 × 7 × 7 → 128
ReLU
Dropout
Linear layer: 128 → 10
Output: digit class logits
```

The CNN is well suited for image recognition because it can learn local features such as edges, curves, and digit strokes.

### 3. Transformer Encoder

The Transformer model treats each image as a sequence of pixels.
134,026 parameters

Architecture:

```text
Input image: 1 × 28 × 28
Flatten image into 784 pixel tokens
Linear pixel embedding: 1 → 128
Add positional encoding
Transformer Encoder layer
Global average pooling
Linear classifier: 128 → 10
Output: digit class logits
```

Since the Transformer processes the image as a sequence, positional encoding is added so the model can learn where each pixel is located in the image.

## Image Augmentation

Four training settings were tested:

| Augmentation Type | Description |
|---|---|
| none | Original MNIST images only |
| rotation | Random rotation up to 10 degrees |
| translation | Random translation up to 10% |
| rotation + translation | Both rotation and translation |

The test set uses only normalization, without random augmentation.

## Training Setup

The models were trained using PyTorch.

Main training settings:

| Setting | Value |
|---|---|
| Optimizer | Adam |
| Loss function | CrossEntropyLoss |
| Learning rate | 0.001 |
| Batch size | 64 |
| Epochs | 5 |
| Random seed | 42 |
| Device | CUDA if available, otherwise CPU |

## Test Results

Final test accuracy on the official MNIST test set:

| Model | No Augmentation | Rotation | Translation | Rotation + Translation |
|---|---:|---:|---:|---:|
| CNN | 99.12% | 99.20% | 99.12% | **99.28%** |
| MLP | 97.77% | **98.03%** | 97.56% | 97.67% |
| Transformer Encoder | 94.42% | **94.75%** | 93.54% | 93.00% |

## Result Discussion

The CNN achieved the best overall performance. Its highest accuracy was **99.28%** with rotation and translation augmentation.

The MLP also performed well, reaching **98.03%** with rotation augmentation. However, because it flattens the image into a vector, it loses the original 2D spatial structure of the image.

The Transformer Encoder reached **94.75%** with rotation augmentation. It worked, but it performed worse than the CNN in this experiment. One reason is that this Transformer implementation treats every pixel as a token, which creates a sequence length of 784. For MNIST, this is less efficient and less natural than using convolutional layers.

## Key Takeaways

- CNN was the strongest model for MNIST digit recognition in this experiment.
- MLP is simple and fast, but it does not preserve image structure.
- Transformer Encoder can be applied to image classification, but this small pixel-level version is not as effective as CNN for MNIST.
- Data augmentation can slightly improve generalization, especially rotation augmentation.
- The best result was achieved by the CNN with rotation and translation augmentation.

## How to Run

Open the notebook in Google Colab or Jupyter Notebook:

```text
MNIST.ipynb
```

Then run the cells from top to bottom.

The notebook will:

1. Import the required libraries.
2. Define the MLP, CNN, and Transformer Encoder models.
3. Load the MNIST training and test datasets.
4. Train each model using different augmentation settings.
5. Evaluate each trained model on the official MNIST test set.
6. Print the final test accuracy.

## Requirements

The notebook uses the following main libraries:

```text
torch
torchvision
numpy
```

If running locally, install them with:

```bash
pip install torch torchvision numpy
```

## Author

Zhongling Ye

## Course

Artificial Intelligence  
Spring 2026
