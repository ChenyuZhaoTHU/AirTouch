# AirTouch


### Operating System: 
    Ubuntu 20.04(Recommanded) and 22.04.


# How to install, build, and implement the system
### Step 1 (Only need once): Installing and Setup for Development Computers
#### 1.1 Install compiling tools
```bash
sudo apt-get install make gcc-arm-none-eabi
```
#### 1.2 Prepare the USB rules for flashing and connecting using Crazyradio (To sovle the permission issues of USB)
```bash
# The following steps make it possible to use the USB Radio and Crazyflie 2 over USB without being root.
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

### Step 2: Pull and clone compiling dependencies for crazyflie fiemware so it can be built.
```bash
cd AirTouch/crazyflie_firmware_constant_height
# Update the submodules, it takes several minutes.
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
Please make sure the building completely seccessed.