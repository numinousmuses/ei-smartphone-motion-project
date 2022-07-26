# ei-smartphone-motion-project

This was a project to play around and get a first feel of edge impulse, and tbh it's nicer than expected.
This project uses an accelerometer to classify between:
* left-right motion
* up-down motion
* circular motion
* idle

The data was collected and deployed on a smartphone, so it's recommended that if you plan to apply the project to a microcontroller you generate your own data. View the complete project on Edge Impulse [here](https://studio.edgeimpulse.com/public/124439/latest).

As for this repository, it contains the neural networks, engineered feature data, and some visualizations.

# project description

## data
First, 20 10-second samples of data for each of the classes (up-down, left-right, circle, idle) were collected. The data are spectra representing the x, y and z axis:

![raw accelerometer spectra](https://user-images.githubusercontent.com/103385201/181065301-9dea1ee1-5a80-4c43-9462-2a7d9f4ed670.png)

Raw accelerometer spectra for each of the motions. Notice the spectra for left-right and up-down and how they represent displacement across a singular axis. Granted, there are small displacement in the other axes due to the accelerometer being handheld during data collection, which is beneficial because the model was intended for deployment into handheld devices as a "magic wand." 

Next, the data was split train/val/test 60/20/20.

Then, feature engineering was applied to the accelerometer spectra primarily to reduce the size of input data to the model, as processing the complete spectra would be computationally intensive, and since the model was intended to be deployed on microcontrollers and smartphones, smaller models were preferred.

For feature engineering, the fast fourier transform was calculated on 2 second pieces of a spectrum. Then, the spectral power was determined and these are the features which would be passed through the neural network. This was all done in Edge Impulse.

![image](https://user-images.githubusercontent.com/103385201/181064124-4bc15c35-9fe2-4bc9-bc64-1623d8175163.png)

Spectral power for idle, circle, up-down, and left-right. This is the visualization of the numerical values stored in the numpy array that is used for training the model. From this we can see how the model will differentiate between the different types of motion based on the extracted features.

## model

The model was made using keras, with an input layer of 33 features (for the 33 extracted features), a dense layer of 20 neurons, a dense layer of 10 neurons, then an output layer of 4 classes.

The code for the network can be found in the notebook under the `neural network` folder. The netron visualizaiton is below. To make the netron visualization, simply download the tfite file fo the model (from the nn folder), go to netron.app, and upload the model.

![image](https://user-images.githubusercontent.com/103385201/181069818-d1843e08-5ab1-485d-a397-410b17a0ec41.png)

The model was trained over 30 epochs, and the results of the training are shown below.

![image](https://user-images.githubusercontent.com/103385201/181070247-69701b86-83de-40f7-8c50-4e5317960cc5.png)

! perfect performance !

## anomaly detection
The anomaly detection model had 32 clusters, and the axes used were those which were highlighted during the feature extraction as the best features to differentiate the classes.

![image](https://user-images.githubusercontent.com/103385201/181071217-3957f789-2397-441f-a0df-4d6abaf73a2c.png)

## test the model
Test results:

![image](https://user-images.githubusercontent.com/103385201/181071910-82d5fa9c-f974-4507-bfab-2418700e6487.png)

## model and implementation analysis
The model needs 2 seconds of data to classify motions. This means that implementing the model on a smartphone or microcontroller means that the motion must be conducted for 2 seconds before a new motion could be used. Despite the model being very accurate, this could be a limitation depending on the intended implementation. For example, if the implementation were a microcontroller in charge of monitoring a motor's maintenance state through vibrations, a 2 second collection period would work. However, if the implementation were to be an interactive game that turned your smartphone into a magic wand, data collection would need to be almost instantaneous, so a 2 second collection period would not suffice. ALl in all, this project was good for getting an overview of Edge Impulse and TinyML

# project ideas
## projects ideas inspired by this project, added to my project bucket list
When I complete projects, I like to list more projects which could be completed based on what I learned while executing the current project. Hyperlinked ideas are ones that I have started or finished, and lead to the project repos.
Here are some ideas:
* industrial maintainance of machines using accelerometer tracker
* magic wand game using accelerometer tarcker
* assistant robot/maintenance module for a drone/autonomous vehicle using accelerometer tracker
* any audio classification project, but using Edge Impulse to deploy to a phone or microcontroller
* [edge impulse keyword detection project](https://github.com/numinousmuses/ei-keyword-spotting-project)
* any video classification project, but using Edge Impulse to deploy to a phone or microcontroller



# credits

Credits to the Introduction to Embedded Machine Learning course by Edge Impulse for the introduction to EI.
