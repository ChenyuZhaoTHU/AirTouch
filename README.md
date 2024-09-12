# AirTouch


### Operating System: 
    Ubuntu 20.04(Recommanded) and 22.04.
### Devices needed:
1. [Crazyflie 2.1](https://www.bitcraze.io/products/old-products/crazyflie-2-1/)

2. [AI-Deck 1.1](https://www.bitcraze.io/products/ai-deck/)
    
3. [Flow deck-v2](https://www.bitcraze.io/products/flow-deck-v2/)

4. [Crazyradio 2.0](https://www.bitcraze.io/products/crazyradio-2-0/) Or [Crazyradio PA](https://www.bitcraze.io/products/crazyradio-pa/)


# Overview
The project contains three main parts, including:
1. Firmware for crazyflie (C code) in _crazyflie_filter_constant_.
    - crazyflie_filter_constant/src/modules/src/range.c

        Revised the height control strategy, to make Crazyflie stay at the same height when passing over an obstacle.

    - crazyflie_filter_constant/example/app_stm_gap8_cpx/src/stm_gap8_cpx.c
    
        The main program of AirTouch excluding neural network module.

2. Firmware for AI-deck (C code) in _/aideck-gap8-inference_.
    - aideck-gap8-inference/examples/ai/classification/classification.c

        Receive data from stm32 on Crazyflie and process neural network inference onboard. 

3. Neural network training (Python code) in _data_and_training_.
    - Sample collected data.
    - Train the neural network based on the collected flight data. Generate the weight file _classification_q.tflight_ with Pruning and Quantization.





# How to install, build, and implement the system
### Step 1 (Only need once): Installing and Setup for Development Computers
#### 1.1 Install compiling tools
```bash
sudo apt-get install make gcc-arm-none-eabi
```
#### 1.2 Prepare the USB rules for flashing and connecting using Crazyradio (To solve the permission issues of USB)
```bash
# The following steps make it possible to use the USB Radio and Crazyflie 2 over USB without being rooted.
sudo groupadd plugdev
sudo usermod -a -G plugdev $USER
```
```bash
# You will need to log out and log in again in order to be a member of the plugdev group. Copy-paste the following in your console, this will create the file /etc/udev/rules.d/99-bitcraze.rules:
cat <<EOF | sudo tee /etc/udev/rules.d/99-bitcraze.rules > /dev/null
# Crazyradio (normal operation)
SUBSYSTEM=="usb", ATTRS{idVendor}=="1915", ATTRS{idProduct}=="7777", MODE="0664", GROUP="plugdev"
# Bootloader
SUBSYSTEM=="usb", ATTRS{idVendor}=="1915", ATTRS{idProduct}=="0101", MODE="0664", GROUP="plugdev"
# Crazyflie (over USB)
SUBSYSTEM=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="5740", MODE="0664", GROUP="plugdev"
EOF
```
```bash
# You can reload the udev-rules using the following:
sudo udevadm control --reload-rules
sudo udevadm trigger
```

### Step 2: Pull and clone compiling dependencies for __crazyflie fiemware__ so it can be built.
```bash
cd AirTouch/crazyflie_filter_constant
# Update the submodules, it takes several minutes.
git submodule init
git submodule update
```
### Step 3: Make and Build the source code of __crazyflie fiemware__
3.1 Restrained by Crazyflie flight control algorithm, when counter height detection, you may change the platform height in _range.c_. 

3.2 The compile the project.

```bash
# Entering the source code for the AirTouch system modules
cd examples/app_stm_gap8_cpx
# Make and build
make -j
```
Please make sure the building completely seccessed.


### Step 4: Pull and clone compiling dependencies for __AI-Deck__
```bash
cd ../../../aideck-gap8-inference
# Update the submodules, it takes several minutes.
git submodule init
git submodule update
```

### Step 5: Compile the AI-Deck firmware
This step is based on trained neural network. The repository contains a trained model _classification_q.tflite_, in the _/model_ folder. Also, you can train your own model based on your data, then replace the model weight file. 
```bash
docker run --rm -v ${PWD}:/module aideck-with-autotiler tools/build/make-example examples/ai/classification clean model build image
```



### Step 6: Flash AI-Deck firmware 
[See more on Crazyflie website](https://www.bitcraze.io/documentation/repository/aideck-gap8-examples/master/infrastructure/flashing/)

If you want over-the-air firmware updates, you can use the cfloader by running the following command.
```bash
$ cfloader flash /module/examples/ai/classification/BUILD/GAP8_V2/GCC_RISCV_FREERTOS/classification deck-bcAI:gap8-fw -w [CRAZYFLIE_URI]
```
You find the binary (after you built your code) under BUILD/GAP8_V2/GCC_RISCV_FREERTOS/target.board.devices.flash.img. An example for the CRAZYFLIE_URI is radio://0/80/2M/E7E7E7E7E7.


### Step 7: Control the Crazyflie on PC and perform detection tasks 
There is an Out-of-Box control program: _crazyflie_filter_constant/control/imu+fly_goto.py_. You can control the drone and carry out edge detection tasks.

# Model Training
You can just follow the code 
