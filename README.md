# CIFAR-10 Image Classification with PyTorch CNNs

This assignment explores image classification on the CIFAR-10 dataset using Convolutional Neural Network (CNN) architectures in PyTorch.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/maliarabaci/tutorial/blob/main/cifar10_tutorial.ipynb)

---

> [!IMPORTANT]  
> **Baseline Reset Rule**: For each experiment in **Steps 2 through 7**, you must return to the **Initial Baseline Configuration (Step 1)** before applying the specified modification. Do **NOT** stack changes cumulatively. Evaluating each step in isolation against the baseline allows you to determine whether each individual modification impacts performance **positively** or **negatively**. You will combine the successful modifications in **Step 8**.

---

## Assignment Instructions

### 1) Initial Baseline Configuration
Run the notebook as initially configured (`cifar10_tutorial.ipynb`).
- **Architecture**: 2 Conv layers (`Conv1`: 3 $\to$ 16, `Conv2`: 16 $\to$ 32, kernel size 4, padding 1), `MaxPool2d(2)`, 2 Fully Connected layers (`FC1`: $32 \times 7 \times 7 = 1568 \to 128$, `FC2`: $128 \to 10$).
- **Hyperparameters**: Adam optimizer ($\text{lr} = 0.01$), `ReLU` activations, `CrossEntropyLoss`, batch size 256, 50 epochs.
- **Task**: Plot the learning curve (Epoch vs. Training Loss) and record the final validation loss and accuracy. This serves as your **baseline benchmark**.

---

### 2) Optimizer Comparison (Adam vs. SGD)
*(Reset to Initial Baseline Configuration before starting)*
- **Modification**: Change the optimizer from Adam to Stochastic Gradient Descent (`torch.optim.SGD`).
  - **Part A**: Run with plain SGD ($\text{lr} = 0.01$, momentum = 0).
  - **Part B**: Tune SGD by adding momentum (`torch.optim.SGD(net.parameters(), lr=0.01, momentum=0.9)`).
- **Task**: Plot the learning curves and record the final validation accuracy and loss for both Part A and Part B.
- **Analysis**: Compare the performance against the Adam baseline. Note the negative impact of un-tuned plain SGD versus the positive recovery achieved by adding momentum (`momentum=0.9`).

---

### 3) Activation Function Impact (ReLU vs. Sigmoid)
*(Reset to Initial Baseline Configuration before starting)*
- **Modification**: Change all activation functions in the network from `ReLU` to `Sigmoid` (`activation_func = nn.Sigmoid()`).
- **Task**: Plot the learning curve for training data, and compute validation accuracy and loss values for the last epoch.
- **Analysis**: Compare the results with the baseline. Discuss the negative impact on convergence speed and accuracy caused by vanishing gradients when using Sigmoid activations in CNNs.

---

### 4) Convolutional Kernel Size (Kernel Size 4 vs. 6)
*(Reset to Initial Baseline Configuration before starting)*
- **Modification**: Increase the kernel size of both convolutional layers from 4 to 6 (`kernel_size_val = 6`).
  - *Note*: You must adjust the spatial dimensions and FC input size accordingly. For $32 \times 32$ input with kernel size 6 and padding 1:
    - After `Conv1` ($K=6, P=1$) and `MaxPool2d(2)`: output spatial size is $14 \times 14$.
    - After `Conv2` ($K=6, P=1$) and `MaxPool2d(2)`: output spatial size is $5 \times 5$.
    - Update `FC1` input dimension in `__init__` and `forward` to $32 \times 5 \times 5 = 800$.
- **Task**: Plot the learning curve, record the final validation accuracy and loss, and note the change in total trainable parameters.
- **Analysis**: Discuss the impact of using larger receptive fields on feature resolution and performance.

---

### 5) Role of Pooling Layers (Removing Max-Pooling)
*(Reset to Initial Baseline Configuration before starting)*
- **Modification**: Remove both `MaxPool2d` layers from the network.
  - *Note*: Without pooling, spatial dimensions remain larger ($32 \times 32 \to 31 \times 31 \to 30 \times 30$). Update `FC1` input dimension in `__init__` and `forward` to $32 \times 30 \times 30 = 28,800$.
- **Task**: Plot the training learning curve and measure final validation accuracy and loss.
- **Analysis**: Evaluate the negative impacts of removing pooling (e.g., sudden parameter explosion from ~200k to ~3.6M parameters, severe computational overhead, and overfitting).

---

### 6) Network Depth (Adding a 3rd Convolutional Layer)
*(Reset to Initial Baseline Configuration before starting)*
- **Modification**: Add a 3rd Convolutional Layer to the model (`Conv3`: 32 $\to$ 64 channels, kernel size 3, padding 1) followed by `MaxPool2d(2)`.
  - *Note*: Update `FC1` input dimension in `__init__` and `forward` to $64 \times 3 \times 3 = 576$.
- **Task**: Plot the learning curve and record the final validation loss and accuracy.
- **Analysis**: Compare the performance with the baseline. Discuss the positive effect of increasing network depth and feature abstraction capacity.

---

### 7) Model Capacity & Width (Increasing Channel Sizes)
*(Reset to Initial Baseline Configuration before starting)*
- **Modification**: Expand the channel capacities and fully connected layer sizes:
  - Increase `Conv1` output channels to 32 and `Conv2` to 64.
  - Increase `FC1` output size to 1024 and `FC2` to 256.
  - *Note*: Update `FC1` input dimension to $64 \times 7 \times 7 = 3136$.
- **Task**: Plot the learning curve and record the final validation accuracy and loss.
- **Analysis**: Compare performance with the baseline. Evaluate the positive impact of increasing channel width and representation capacity on validation accuracy.

---

### 8) Final Architecture Synthesis & Optimization
*(Combine promising methods from previous steps)*
- **Task**: Combine the beneficial modifications identified in Steps 2–7 (e.g., optimal depth/width, activation functions, pooling, optimizer, learning rate schedule, data augmentation, etc.) to construct your final optimized CNN architecture.
- **Report**: Present your final architecture diagram/summary, learning curves, and final test set accuracy. Aim to achieve the highest possible classification performance!
