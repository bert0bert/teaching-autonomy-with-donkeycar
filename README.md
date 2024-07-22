# Self-Driving Cars with Donkeycar

This experiment utilizes Donkeycar, the "Hello World" of self-driving cars. This experiment will teach you the basics of self-driving cars and how to use them with small RC cars. 

It should take 2-3 hours to run through this experiment.

You can run this experiment on the [Cloudlab](https://cloudlab.us/) testbed. To run this experiment you should already have an account on cloudlab, be part of a project, and have access to a small-lan profile.

## Background

### Donkeycar

Donkeycar is an open-source self-driving platform that allows people from various backgrounds utilize self-driving in their Remote Controlled (RC) cars without having to deal with low-level details. The reason we use Donkeycar is because of it's high-level of abstraction and how easy it us to use in an experiment. It's a great way to close the gap between the hardware of your car and machine learning implemented in python, that way the experimenter doesn't need to worry about writing code.

### End to End Deep Learning for Autonomous Vehicles

The deep learning autopilot uses a single forward facing camera and a convolutional neural network (CNN) to implement an autopilot technique called "Behavioral Cloning" (or Imitation Leaerning). The goal is to create an autopilot that imitates that actions of a human driving the car. The process is as follows:

A human manually drives around a track to collect data, Donkeycar records this data at 20 times per second, each piece of data has three components: a camera image, a throttle value, and steering value. All of these values captured at the same time for each camera image. The data is then cleaned as to make sure a machine learning model doesn't learn to replicate a crash a steering off course. Finally, using the data collected we train a Convolutional Neural Network. Using the trained CNN to infer throttle and steering values given an image, we can now run the model on the simulator in a similar track. Whenever an image is taken and is given to the CNN the cars harrdware is then given a thorttle and steering value; that is done 20 times per second. 

Note: Since the deep learning autopilot depends on camera image, lighting conditions are important so in a controlled environment where lighting can be controlled this deep learning template is very effective, but when done outside it can be more difficult.

## Experiment Methodology

This experiment is specifically meant to teach students how to use donkeycar in a virtual environment using donkeygym. So using a cloudlab instance, we will be learning how download and install donkeycar, run the donkeygym simulator to collect data, clean data, train a machine learning model, and use that machine learning model to run a self-driving car within our simulator. 

### Downloading Donkeycar to Cloudlab server

Our first step is to download Donkeycar to our cloudlab server

```
# get donkeycar
mkdir ~/projects
cd ~/projects
git clone https://github.com/autorope/donkeycar.git
cd donkeycar
git checkout main
sudo python3.11 -m pip install -e .[pc] 
```
### Getting donkeygym
Donkeygym is the simulator that we will use to collect data to train the model that will make our RC car self-driving

```
# get gym-donkeycar
cd ~/projects
git clone https://github.com/tawnkramer/gym-donkeycar.git
cd gym-donkeycar
sudo  python3.11 -m pip install -e .[gym-donkeycar]
```

### Creating Donkey app

Now in order to run the simulator we have to create the application for it. We do it by running the following:

```
# create donkey app
cd ~/projects
donkey createcar --path mycar
cd mycar
mv myconfig.py myconfig.py.bak
cat > myconfig.py << EOF
DONKEY_GYM = True
DONKEY_SIM_PATH = "remote"
SIM_HOST = "127.0.0.1"
DONKEY_GYM_ENV_NAME = "donkey-generated-track-v0"
GYM_CONF = { "body_style" : "car01", "body_rgb" : (255, 0, 0), "car_name" : "nyu", "font_size" : 100}
EOF
```

### Running Donkeygym

TBD

### Cleaning data

In order to train a machine learning model we have to make sure we have a "clean" dataset, which means none of our data contains images of crashes or going through any objects. Still within the VNC window run the following on a terminal:
```
donkey UI
```

Within the donkey UI, select 'Tub Manager" on the top left corner. Once it finishes loadingg select 'Load Config' and until you get to your mycar director. If done correctly, when pressing on 'Load Tub' you can then select your dataset name (in this case I chose the name of my dataset to be 'mydata'). It'll load all your data where you can look through and delete.

To delete data, start from right before you crash your car and press the button 'Set left', then look for right when you start to drive back on track and then press 'Set right'. Then you can delete and reload tub. Note: if you forget to set a left or a right, it will delete everything from the starting point to where you set right or from wherever you set left until the ending point of your data set. Make sure you select a 'set left' and 'set right'. Your dataset automatically saves when you delete and reload tub.


### Configuring Cloudlab for Training

We're now going to train a model using the dataset we collected. 
```
cd donkeycar
git checkout main
python3.11 -m venv .venv
source .venv/bin/activate
pip install -e .[pc] tensorflow==2.12
donkey createcar --path ~/mycar
```

### Training model

Using the data we collected and now have cloudlab configured, we are now going to train a model.
First check and see if DEAFAULT_MODEL_TYPE = 'linear'
```
cd ~/mycar
nano myconfig.py
```

The line should look like this: 
```
DEFAULT_MODEL_TYPE = 'linear'
```

Now it's time to train the model.
```
cd mycar
donkey train --tub ./data/[data subdirectory] --model ./models/<model_name>.h5
```

### Running model
TBD

## References
