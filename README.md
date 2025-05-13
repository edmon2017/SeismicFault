# Seismic Fault Interpretation

Reflection Seismic is a geophysical method used for subsurface exploration in the oil and gas industry. This method is based on the analysis of seismic waves recorded after these has traveled trough the subsurface rocks.

Part of the key information preovided for this method is the location of geological faults, which are zones were the rocks has been broken and displaced. The identification of the faults is a key taks because these can related to hydrocarbon acumulations, and also becaouse the faults can be associated to hazards for the drilling operations. 

However the fault interpretation, is a complex task, wich relies on the interpreter experience and consumes a lot of time.

The goal for this project is to automatize the fault interpretation process, to save time and reduce the subjectivity of this process.

The data for this project is from the Force-2020-Machine-Learning-competition at this link:

https://github.com/bolgebrygg/Force-2020-Machine-Learning-competition

The first step is to import the data. Seismic information is recorded in a format called SEGY, so I need to create a function to read this format and convert it into a 3D array. For this step, I'm going to use the library SEGYIO. Then, I used that function to read two volumes: one with seismic data and another with fault labels

## Exploratory Data Analysis
To perform the EDA, it is necessary to display the seismic information, to inspect the data, and to perform a visual QC. For this step, I create a function to display the seismic data.
Next is an example of an inline, where we can observe both volumes, the seismic amplitude, and fault labels. For the first volume, I can observe that there are no missing traces, and there is data for the entire time window. For the second volume, I can observe the data correspond to the same range on xlines and to the same time window. In addition, we can observe the fault position is indicated with a value of 1 and 0 where there are no faults. After this inspection of the data, I can conclude the data is ok and ready to be used.

![image](https://github.com/user-attachments/assets/2af1b377-2515-48d5-a929-c912a6e6b884)

![image](https://github.com/user-attachments/assets/dd68c9ce-5452-4bc6-9b48-8ad45e0a2a9f)

Next, I'm going to split the data in two subvolumnes for training and testing based on the xline range. The total number of xlines is 589, the training datraset will be a subvolume between the first and 400 xline, and the testing dataset between the 401 and the last xline.


## Model creation
For this project, I created a model using PyTorch. This model uses a convolutional neural network.

I created a class SEGYDataset to train the model to extract 3D patches from the volume. I'm going to use 1000 patches, and these are going to be normalized before training.

Then, I'm going to implement a U-Net architecture for binary segmentation.

For the last step, I will train the network using BCELoss, which is used for binary classification, and Adam for optimization.

The next plot shows the Loss function from the training in the previous step, we can notice there was no much change beyong the 20 epoch

![image](https://github.com/user-attachments/assets/c93000ce-348e-4348-b7bf-0dad771d350d)


## Hyperparameter tuning
In this step, I'm going to optimize the hyperparameters, for this taks I'm going to calculate the Dice coefficient for image segmentation, between train and validation data.

First, I define two functions to evaluate the epoch and calculate the Dice coefficient, and then perform the hypermarameter search considering different patch size, batch size and learning rate. For this evalution a use only 5 epochs to make the process faster.


![image](https://github.com/user-attachments/assets/cc22af72-2dcc-454b-9960-0e15a658f923)

## Result and Analysis
To analyze the performance of the network, I use the testing set that create at the beginning. First, I defined a function to calculate the Dice, the result was 0.54, which is a value good enough for this problem.

![image](https://github.com/user-attachments/assets/111c08c3-141f-4c66-8965-b5e6025f9127)

![image](https://github.com/user-attachments/assets/327a5535-b6b7-477f-b416-38c68738f360)

![image](https://github.com/user-attachments/assets/0b90cfad-3080-467b-b450-240c4e578195)

## Conclusions
* For this project I use a convolutional neural network using a U-Net architecture, to interpret faults from seismic data.
* The performance of the network was measured based on the Dice coefficient between predicted and actual labels.
* Results are good enough to consider this as an effective approach to reduce the time required for a seismic interpretation.
* A future work could be extend the training of this CNN to include patches from other seismic volumes with a different geological configuration and signal to noise ratios.



