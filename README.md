# RSNA Intracranial Hemorrhage Detection

This project aims to develop a model capable of detecting various types of intracranial hemorrhages (ICH) from CT scans using deep learning techniques. The dataset used for this project is sourced from the RSNA Intracranial Hemorrhage Detection competition.

## Key Innovations and Novelty
The project demonstrates a modern approach to multi-label classification for medical image analysis, specifically focused on the following aspects:

- **Balanced Sampling**: We address the class imbalance problem inherent in medical datasets by ensuring equal sampling from each hemorrhage subcategory. This ensures that the model does not become biased toward the more frequent classes.
  
- **Custom Dataset Class**: A custom `RSNADataset` class is implemented to handle DICOM images efficiently. The dataset class takes care of both image loading and label extraction, with robust handling of multi-label classification.

- **Multi-Label Classification**: A simple yet effective CNN architecture is employed for multi-label classification, where each image can belong to multiple hemorrhage subcategories simultaneously. The model uses the `BCEWithLogitsLoss` loss function for binary cross-entropy, which is well-suited for multi-label tasks.

- **Preprocessing and Data Augmentation**: Standard preprocessing techniques such as resizing and normalization are used. Images are resized to 224x224 pixels for compatibility with the CNN architecture. The model benefits from being trained with well-optimized image preprocessing pipelines.

- **Model Architecture**: A lightweight CNN architecture is implemented, featuring two convolutional layers followed by fully connected layers. This architecture is efficient yet effective for the task of hemorrhage classification.

- **Evaluation Metrics**: We evaluate the model performance using accuracy, precision, recall, F1-score, and confusion matrix to understand both overall and per-class performance.

- **Training with Validation**: The model is trained on 80% of the data with validation on 10% of the data. The best-performing model is saved for further evaluation on the test dataset.

- **Interpretability**: For each prediction, the model outputs both the predicted subcategories and their corresponding probabilities. Additionally, the image is displayed along with the predicted category and probability, improving the interpretability of the results.

## Algorithm and Methods

### Data Preprocessing and Augmentation

- **Balanced Sampling**: We implement a method for equal sampling from each hemorrhage subcategory. This ensures the model doesn't suffer from class imbalance, which is common in medical image datasets.
  
- **Custom Dataset**: A PyTorch Dataset class is defined to handle image loading and label extraction efficiently. This class reads the DICOM images, converts them to a 3-channel RGB format, and applies the necessary transformations.

### Model Architecture: SimpleCNN

The model follows a simple convolutional architecture with two convolutional layers followed by two fully connected layers:

- **Conv1**: 3 input channels (RGB image) → 16 output channels (feature maps) using a kernel size of 3x3.
- **Conv2**: 16 input channels → 32 output channels.
- **Fully Connected Layers**: After flattening the output from the convolutional layers, the model passes the data through two fully connected layers to predict the 6 subcategories.

The output layer uses 6 neurons, each representing a subcategory of hemorrhage, where the model predicts the probability of each class using a sigmoid activation function.

### Loss Function and Optimization

- **BCEWithLogitsLoss**: This loss function is ideal for multi-label classification as it computes the binary cross-entropy loss for each label independently.
- **Adam Optimizer**: We use the Adam optimizer with a learning rate of 0.001 to update the model weights.

### Training Procedure

The training process involves:

1. Loading the dataset and applying necessary transformations.
2. Training the model for 5 epochs, with each epoch consisting of forward passes, loss calculation, backpropagation, and weight updates.
3. The validation set is used to monitor performance, and the model is saved if the validation loss improves.

### Evaluation

After training the model:

- We evaluate the performance on the test set using accuracy, precision, recall, and F1-score, as well as the confusion matrix to analyze the model's performance in predicting each hemorrhage subcategory.
- A detailed confusion matrix and classification report are provided for each subcategory.

### Prediction and Results

For any input DICOM image, the model outputs the predicted subcategories along with the probabilities. The image is displayed, and the model predicts the class with the highest probability (excluding "any" - Any is well trained to capture hemorrage it is always 100%, otherwise 0% for Normal- This shows how much the model learned well).

## Conclusion

The model demonstrates high performance in detecting various types of intracranial hemorrhages from CT scans. The use of a balanced dataset, efficient architecture, and multi-label classification methods contribute to its success in this challenging medical imaging task.

## Requirements

- PyTorch
- torchvision
- pydicom
- numpy
- pandas
- scikit-learn
- seaborn
- matplotlib

## Usage

### Load the Pre-trained Model
The model can be loaded by running:

```
model.load_state_dict(torch.load('best_model.pth'))
```

### Make Predictions
To make predictions, provide the path to a DICOM image, and the model will output the predicted subcategories along with probabilities.

```
image_path = '/path/to/dicom_image.dcm'
predictions, probabilities, img = predict_image(image_path)
```

### Acknowledgments
- RSNA Intracranial Hemorrhage Detection dataset
- PyTorch for deep learning framework
- pydicom for handling DICOM images
