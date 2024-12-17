# Jun 21 - Jul 5 Report

### KCL 2024 Summer Internship @Yifei Shi

---

### 1. Outlines

##### 1.1. Review on the tasks

- Pruning for ResNet models basing on ViTac datasets. 

##### 1.2. Experiment: ResNet Model Training

- ViTac dataset loading and pre-process. 
- Model training for visual dataset. 
- Model training for tactile dataset.
- Pruning and evaluation. 

##### 1.3. Next Step

### 2. Review on the tasks

##### 2.1. Main Taks

- Training models for **ViTac datasets** classification, for both of the visual and tactile data. 
- Application for **ResNet** model, with proper evaluation. 
  - Loss value
  - Accuracy on the validation set
  - Time duration for training and evaluation
- **Prunning** and then evaluate, followed by the conclusion last week. 

##### 2.2. CREATE HPC Access

- Since training vision models need GPU resources, access to the server will be helpful. 
- Attempt to access but still <u>cannot establish the connection</u> currently. 
  - My e-Research account could access your group. (Many thanks for your assistance this week. ) But cannot authorize my ssh connection request.
    - i.e., After entering `ssh <k-num>@bastion.er.kcl.ac.uk`, it gives an alert said, 
      *You may need to authenticate your SSH access by visiting the e-Research Portal:*
      *https://portal.er.kcl.ac.uk/mfa/*
      But sometimes there is the entry of my request, most cases there is no update, and my request will be denied. I could approve with that. 
    - After entering `ssh <k-num>@hpc.create.kcl.ac.uk` or `ssh -J <k-num>@bastion.er.kcl.ac.uk <k-num>@hpc.create.kcl.ac.uk` , there is no request from that website, altough I approve my request to the bastion, but still cannot access to the hpc as I cannot approve with my request this time. 
  - The .pdf document provide some roughly intrudoctuion, but the section on the documentation page has been deleted now.  
  - I will have a try on connect with eduroam, or contact with the help desk later for addressing with this. 
- Still performed the experiments on my device, but fortunately got some outcomes. 

### 3. Experiment

##### 3.1. General Idea

- Train **ResNet** model basing on visual & tactile datasets, pick the model with the best performance. Then perfrom pruning, observe the improvement. 

  - This time I used **resnet18** at first, to see whether a shallow neural network could achieve a good performance. 

- Read through the readings about ResNet to learn some conceptual knowledge. *Ref: Deep Residual Learning for Image Recognition (K. He et al., 2015)*

  <img src="/Users/kiyoaki/Library/Application Support/typora-user-images/image-20240705170551101.png" alt="image-20240705170551101" style="zoom:50%;" />

- Implemented a better framework for model loading, training, evaluation and pruning this time. Used tensorboard for reading the data. 

##### 3.2. Data Preprocess

###### 3.2.1. Dataset Class implementation

- Dataset
  - Visual Data
    - Take the index of folder name as the label. e.g. `vis0011` indicates <u>11</u>. 
    - Read the train data and test data from .txt files directly. 
  - Tactile Data
    - Samilar process for the label. e.g. `CD0101` indicates <u>101</u>. 
    - Read the train data and test data from .txt files directly. 
    - Only process the data from "press" action. 

###### 3.2.2. Normalization

- Approach ([*Ref: J. Lee et al., 2019*](https://github.com/jettdlee/vis_tac_cross_modal))

  ```python
  IMAGENET_MEAN = [0.485, 0.456, 0.406]
  IMAGENET_STD = [0.229, 0.224, 0.225]
  
  transforms.Resize((227, 227))
  transforms.ToTensor()
  transforms.Normalize(mean=IMAGENET_MEAN, std=IMAGENET_STD)
  
  ```

  ```python
  train_dataloader = DataLoader(train_set, batch_size=batch_size, num_workers=2, shuffle=True)
  test_dataloader = DataLoader(test_set, batch_size=batch_size*2, num_workers=2, shuffle=True)
  
  ```

##### 3.3. Visual data - Model training

###### 3.3.1. Model Configuration

- Configuration
  - Network - **ResNet18** 
  - Loss function - **CrossEntropyLoss**
  - Optimizer - **Adam**
  - Scheduler - **OneCycleLR**
- Hyper parameters
  - learning_rate = ?  # Pick the optimal lr from (1e-2, 3e-2, 1e-3)
  - n_epochs = 30
  - batch_size = 64

###### 3.3.2. Training and evaluation

- Comparison

  > <img src="/Users/kiyoaki/Library/Application Support/typora-user-images/image-20240705174414868.png" alt="image-20240705174414868" style="zoom:40%;" />
  >
  > learning_rate = 3e-2 (red)
  >
  > learning_rate = 1e-2 (blue)
  >
  > learning_rate = 1e-3 (yellow)
  >
  > > LR indicates max lr for scheduler

  - An optimal learning rate: 1e-3

    |      | 3e-2          | 1e-2            | 1e-3                |
    | ---- | ------------- | --------------- | ------------------- |
    | Acc  | 0.940         | 0.932           | **0.975**           |
    | Loss | 0.225         | 0.217           | **0.128**           |
    | Time | 4100s (1.1hr) | 4121s (1.106hr) | **3989s (1.085hr)** |

- Evaluation (as lr = 1e-3)

  - Accuracy: ~97% (Converage around 23 epoch)
  - Loss: ~0.1284
  - Time duration for training: 3989s (~67 mins)
  - Time duration for evaluation: ~28.89s
  - Number of valid weights: 11,683,712

##### 3.4. Visual data - Model pruning and evaluation

###### 3.4.1. Unstructured Global Pruning

- Iteratively execute pruning to get the optimal amount.

  > ![image-20240705183027124](/Users/kiyoaki/Library/Application Support/typora-user-images/image-20240705183027124.png)
  >
  > An ideal amount for this case might be **0.4**

  - Conclusion (as amount = 0.4)
    - Time duration for evaluation: ~29s
    - Accuracy: 0.9613
    - \#Valid weights = 7,012,147

###### 3.4.2. Structured Pruning

- Execute l1structured prunning for <u>model.fc</u>

  >![image-20240705185023435](/Users/kiyoaki/Library/Application Support/typora-user-images/image-20240705185023435.png)
  >
  >- Perform with the standard model
  >- Samiliar method like previous unstructured one.

  - Conclusion (as amount = 0.8)
    - Accuracy: 0.1393
    - \#Valid weights: 11,274,112
    - *Didn't improve a lot.* 

- Refer to [CVPR’23: DepGraph: Towards Any Structural Pruning (G. Fang et al., 2023)](https://github.com/VainF/Torch-Pruning) with their providing examples. 

  - Perform with the standard model. 
  - Conclusion
    - Accurancy: 0.8980
    - \#Valid weights: 11,672,510
    - *May need further experiments…*

###### 3.4.3. Composing Structured pruning & Unstructured pruning

- Perform unstructured pruning first, then structured pruning. (a)
  - Conclusion (a) - `amount_unstructured, amount_structured = 0.4, 0.8`
    - Time duration for evaluation: ~28.82s
    - Accurancy: 0.9694 (Loss: 0.1686)
    - \#Valid weights: 6,768,025
- Perform structured pruning first, then unstructured pruning. (b)
  - Conclusion (b) - `amount_structured, amount_unstructured = 0.2, 0.4`
    - Time duration for evaluation: ~28.24s
    - Accurancy: 0.9694 (Loss: 0.1544)
    - \#Valid weights: 7,012,147 (Not effected)
    - *Note: Structured prunning amount cannot be >= 0.3 otherwise the loss will increase rapidly*

##### 3.5. Tactile data - Model training

###### 3.5.1. Model Configuration

- Configuration
  - Network - **ResNet18** 
  - Loss function - **CrossEntropyLoss**
  - Optimizer - **Adam**
  - Scheduler - **OneCycleLR**
- Hyper parameters
  - learning_rate = 1e-2
  - n_epochs = 10
  - batch_size = 64

###### 3.5.2. Training and evaluation

- Evaluation

  ><img src="/Users/kiyoaki/Library/Application Support/typora-user-images/image-20240705183804396.png" alt="image-20240705183804396" style="zoom:25%;" />
  >
  ><img src="/Users/kiyoaki/Library/Application Support/typora-user-images/image-20240705183812928.png" alt="image-20240705183812928" style="zoom:25%;" />

  - Accuracy: ~99% 
  - Loss: ~0.07
  - Time duration for training: 5212s (~87 mins)
  - Time duration for evaluation: 60.82s
  - Total valid weights: 11,683,712

  > Data looks weird, as it achieves nearly 75% accuracy for the 1st epoch of training...

##### 3.6.  Tactile data - Model pruning and evaluation

- Iteratively execute pruning to get the optimal amount

  > ![image-20240705190615643](/Users/kiyoaki/Library/Application Support/typora-user-images/image-20240705190615643.png)
  >
  > An ideal amount for this case might be **0.5**

  - Conclusion (as amount = 0.5)
    - Time duration for evaluation: ~57s
    - Accuracy: 0.9893 (Loss: 0.0613)
    - \#Valid weights = 5,844,256

### 4. Conclusion

- Findings
  - Although I referred some open repo on github this time, the framework implemented by myself may not be the most effective. Some external libs may be used in this context.
  - ResNet18 seems capable for simple classification, but some experiment on ResNet34 may also needed, since current model needs a long time to train. 
  - There may potiential issues on data preprocess stage. Further research is needed. 
  - Some existing research on pruning, especially structured pruning for ResNet is awesome, and could be used in this context as well. 
  - The composing of different pruning methods could be applied, but the order matters. 
- Next step
  - Connect with CREATE HPC
  - Potiential issues on labelling dataset (maybe)
  - Try some more implemented framework (e.g. YOLOv8) 
  - Extend the experiments on Tactile datasets
  - Try on some deeper network structure (e.g. ResNet34)
  - Some further research on the structure of ResNet, so some further pruning can be performed. 

