# CapstoneProjectFinal

Code that creates features for vibration sensor response of cnc machine for failure detection.
This code presents the results of using scikit learn classifier on a data set provided for the vibrational response of a cnc mill.
Three different milling machines are observed over multiple months for thirteen different machining operations.
This work aims to determine what kind of model is most adept at finding failures during operation of the machines and to determine whether viewing the machines as uniqe or the operations as unique is a more applicable approach to solving the problem.

The faults in this data are only labelled as GOOD or BAD. This reduces insight dramatically into the problem as the reason for the "BAD" label is not described. This means that the bad labels may include many things such as a metal chip stuck in the chuck during tool change, a blunt cutting blade, worn machininery, etc. The goal of thie modelling effort is to determine if a model can determine positive or negative outcomes from only the data provided.

All rights to the data set are given to Mohamed-ali tnani : mohamed-ali.tnani@boschrexroth.de and Bosch.
Data sets can be found at the following link from the original author: https://github.com/boschresearch/CNC_Machining
All data sets used can be found in the zip file attached.
# Initial findings
The initial findings for this work indicate high levels of accuracy for predicting failure when observing all operations of a specific machine and using a logistic regression approach and random forest classifier techniques. 
In order to make this data work signal data (frequency from vibration sensors) was used with an initial fourier transform applied to get mathematical decriptors of the signal to be used in modelling efforts.
From this data some basic calculations were done to define the following signal metrics:
1. Spectral centroid
2. spectal flux
3. peak frequency
4. bandwidth

This was done for data for the X, Y and Z mounting directions for the vibration sensors on the back of the spindle of the cnc machine. 
From this a quick kernel density approximation of the different values could be done which indicated some key differences in the signal data of the good and bad outcomes for the machining processes.

# Model description
After experimentation with decision trees, logistic regression and random forest classiers it was found the latter was most effective. From implementing the model the random forest classifier was able to correctly determine faults in 19/21 failed cnc runs despite a class imbalance of more than 800:35 for positive to negative outcomes.

The model was also implemented using only the first 4096 data points of a given machining operation which is useful as this would allow the operator to stop the operation before completion and potentially have the following outcomes:
1. Saved materials
2. Less wear of machining bits.
3. lowered chance of costly catastrophic failure.
4. Higher yield percentage for hours worked.

From the model it was found that z- bandwidth and peak-frequency had all zero values as thier input values were all lower than zero initially. This means that despite this lack of insight into the Z axis behaviour of the machine, the model was still adequate in finding the faults. Further investgation also revealed that the model was minimally impacted by the type of operation being done, that is to say that if OP1 is t-slot cutting and OP2 is end milling, that this was not usefully correlated to the measured data. There was however some reliance of the model on the machine being used with machine 1 and 3 displaying the highest feature importance for the categorical features, followed by maching operations 5, 13 and 10. 
In this case these machining operations are masked and cannot be further described explicity (e.g., milling or turning operations).

# Actions
From this model the following actions can be taken:
1. Implementation of the model in a real time control system coupled to data read from the vibration sensor for the individual machines.
2. Addition of flagging mechanism for operators to visually see the in-situ outcome of the machining operation.
3. Addition of further classes for the classifications used, adding more information such as the type of failure observed by the operator rather than ust OK and NOK. Doing this could allow for further cost saving in the long run.

# Accessing the data
The data set used in this project can be found at :
https://archive.ics.uci.edu/dataset/752/bosch+cnc+machining+dataset
It has not been uploaded due to the size restrictions of github.
