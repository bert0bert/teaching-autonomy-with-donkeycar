# Teaching Autonomy Through Donkeycar

This experiment utilizes Donkeycar, the "Hello World" of self-driving cars. This experiment will teach you the basics of self-driving cars and how to use them with small RC cars.

It should take 2-3 hours to run through this experiment.

You can run this experiment on the [Cloudlab](https://cloudlab.us/) testbed. To run this experiment you should already have an account on cloudlab, be part of a project, and have access to a small-lan profile.

## Background

### Donkeycar

Donkeycar is an open-source self-driving platform that allows people from various backgrounds utilize self-driving in their Remote Controlled (RC) cars without having to deal with low-level details. The reason we use Donkeycar is because of it's high-level of abstraction and how easy it us to use in an experiment. It's a great way to close the gap between the hardware of your car and machine learning implemented in python, that way the experimenter doesn't need to worry about writing code.

## Experiment Methodology

This experiment is specifically meant to teach students how to use donkeycar in a virtual environment using donkeygym. So using a cloudlab instance, we will be learning how download and install donkeycar, run the donkeygym simulator to collect data, clean data, train a machine learning model, and use that machine learning model to run a self-driving car within our simulator. 

### Downloading Donkeycar to Cloudlab server

Our first step is to download Donkeycar to our cloudserver

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
