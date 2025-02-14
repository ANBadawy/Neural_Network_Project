# **Findings Report**

## **Code Review**

The code involved several stages in preparing and experimenting with a time series forecasting model using LSTM (Long Short-Term Memory) neural networks in PyTorch. Here’s a breakdown of the code structure and purpose:

### **Data Preprocessing**:

* The dataset was loaded, and preprocessing steps were applied, including handling missing values, normalizing the data, and creating time-based features.

* The data was split into training, validation, and test sets, allowing for model evaluation across different subsets.

### **Windowed Dataset Creation**:

* A custom class, `WindowGenerator`, was developed to create sliding windows of input sequences and labels from the time series data. This class facilitated the preparation of data for LSTM models by splitting sequences into input and target windows.

* A `TimeSeriesDataset` class was also used to create PyTorch-compatible datasets with batching and shuffling functionality.

### **Model Architecture**:

* Three variations of LSTM models were implemented:
    * **Single-layer LSTM model**: A basic model with a single LSTM layer followed by a linear layer for output.
    * **Two-layer LSTM model**: A deeper model with two LSTM layers to capture more complex temporal patterns.
    * **LSTM with Activation**: A multi-layer LSTM model that included a ReLU activation function after the LSTM layers to enhance non-linearity before the final output layer.
* Each model variant aimed to improve the model’s ability to learn temporal dependencies in the dataset.

### **Training and Evaluation**:

* Multiple training configurations were experimented with, adjusting hyperparameters such as batch size, learning rate, and number of layers.
* The models were trained for a set number of epochs, and loss values were calculated after each epoch.
* A loop with various combinations of hyperparameters was implemented to evaluate each configuration and track the average loss, making it possible to compare their performance across different settings.

### **Parameter Tuning and Visualization**:

* Extensive hyperparameter tuning was performed, with combinations of batch size (64, 128, 256, 512), learning rate (0.001, 0.005, 0.01, 0.05), and number of layers (2, 3, 4, 5).
* The results were plotted to visualize the effect of each parameter combination on model performance, showing average loss across the configurations.

## **Key Findings and Model Comparison**

1.  **Single-layer vs. Multi-layer Models**:

    * The single-layer LSTM model served as a baseline, providing moderate performance with simpler temporal patterns.
    * Moving to a two-layer LSTM model introduced a slight improvement in learning capacity, capturing more complex dependencies in the data. However, gains were marginal, indicating that the dataset’s patterns may not require much depth.
    * The multi-layer models with ReLU activation performed variably depending on the hyperparameters. The activation function added non-linearity, which occasionally helped but also increased the risk of overfitting in deeper configurations.
2. **Impact of Hyperparameters**:

    * Batch Size: Smaller batch sizes (64 and 128) generally resulted in lower losses across models. This may be because smaller batches allow more updates per epoch, leading to finer adjustments in the weights.
    * Learning Rate: A learning rate of 0.001 consistently resulted in the best performance, allowing the models to converge steadily. Higher learning rates, especially 0.05, caused instability and increased losses, particularly in deeper models and larger batch sizes.
    * Number of Layers: The two-layer and three-layer configurations achieved lower losses compared to four-layer or five-layer models. Adding too many layers appeared to increase model complexity without significantly enhancing the model’s ability to learn from this dataset, often resulting in higher losses.
3. **Model Performance Trends**:

    * Models with 2 or 3 layers, a batch size of 64 or 128, and a learning rate of 0.001 demonstrated the best performance, indicating that these configurations balance complexity and generalization.
    * Increasing batch size and learning rate often led to higher average losses, suggesting that the model might struggle with larger data chunks and faster updates.
    * Deeper models (4 or 5 layers) and higher learning rates introduced noise and divergence, with occasional spikes in loss. This suggests that simpler architectures may generalize better on this dataset.
4. **Best Configuration**:

    * The optimal configuration across all models involved a batch size of 64, a learning rate of 0.001, and 2–3 LSTM layers. These settings resulted in lower average losses on the validation set, providing the most stable and generalizable performance across the tested configurations.

## **Conclusion**:
The experiments showed that simpler LSTM configurations were more effective for this time series forecasting task. A single-layer LSTM captured basic patterns, while a two-layer model provided modest improvements. Adding ReLU activation helped slightly, but deeper models with high learning rates often led to higher losses, indicating overfitting. Overall, a two- or three-layer LSTM with a learning rate of 0.001 and smaller batch sizes performed best, highlighting the value of simplicity and careful tuning over excessive model complexity.