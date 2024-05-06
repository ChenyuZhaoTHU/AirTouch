# AirTouch


### Operating System: 
    Ubuntu 20.04(Recommanded) and 22.04.


# How to install, build, and implement the system
### Step 1: Install compiling tools
```bash
sudo apt-get install make gcc-arm-none-eabi
```


### Step 2: Pull and clone compiling dependencies for crazyflie fiemware so it can be built.
```bash
cd AirTouch/crazyflie_firmware_constant_height
# Update the submodules
git submodule init
git submodule update
```
### Step 3: Make and Build the source code
```bash
# Entering the source code for the AirTouch system modules
cd examples/app_stm_gap8_cpx
# Make and build
make -j
```
Please make sure the building seccessed.