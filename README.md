# FaceRecognitionRaspi3B-
Playing with my Raspberry Pi 3B+ using OpenCV

#### Source:
##### https://pyimagesearch.com/2017/09/04/raspbian-stretch-install-opencv-3-python-on-your-raspberry-pi/
##### https://docs.opencv.org/4.x/d7/d9f/tutorial_linux_install.html

## Preparation
### Step 1:
#### Test the camera works:
```
libcamera-still -o test.jpg
```

### Step 2

#### Expand filesystem
```
sudo raspi-config
```
and then expand filesystem

```
sudo reboot
```

### Step 3
#### Install dependencies
```
sudo apt-get update && sudo apt-get upgrade
```

### Step 4
#### Install some developer tools including CMake
```
sudo apt update && sudo apt install -y cmake g++ wget unzip
sudo apt-get install build-essential cmake pkg-config
sudo apt-get install libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev
sudo apt-get install libjpeg-dev libtiff5-dev
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
sudo apt-get install libxvidcore-dev libx264-dev
sudo apt-get install libatlas-base-dev gfortran
sudo apt-get install python3-apt
sudo apt-get install python3-distutils
```

### Step 5 
#### Know how much space we used:
```
df -h
```

```
Filesystem      Size  Used Avail Use% Mounted on
/dev/root        29G  2.0G   26G   8% /
devtmpfs        325M     0  325M   0% /dev
tmpfs           455M     0  455M   0% /dev/shm
tmpfs           182M  980K  181M   1% /run
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
/dev/mmcblk0p1  255M   32M  224M  13% /boot
tmpfs            91M     0   91M   0% /run/user/1000
```

## OpenCV
### Download the source code
```
wget -O opencv.zip https://github.com/opencv/opencv/archive/4.x.zip
unzip opencv.zip
```

### We also need to grab the opencv_contrib repository
```
wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.x.zip
unzip opencv_contrib.zip
```

## Python installation
### Install pip
```
wget https://bootstrap.pypa.io/get-pip.py
sudo python3 get-pip.py
```

### Install a virtual environment
```
sudo pip install virtualenv virtualenvwrapper
sudo rm -rf ~/.cache/pip
```

### nano ~/.profile
### Add the following lines at the bottom of the file:
```
# virtualenv and virtualenvwrapper
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
source /usr/local/bin/virtualenvwrapper.sh
```

### Create the Python virtual environment
```
source ~/.profile
mkvirtualenv cv -p python3
```

### To use it
```
source ~/.profile && workon cv
```

### Install numoy
```
pip install numpy
```

### Setup our build using CMake
Inside the cv environment:
```
cd ~/opencv-4.x/
mkdir build
cd build
```

```
sudo cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib-4.x/modules \
    -D BUILD_EXAMPLES=ON ..
```

! The two dots at the end are apparently important, if I removed them, I received the error: "source" does not appear to contain CMakeLists.txt.

### Increase the swap space size
This enables OpenCV to compile with all four cores of the Raspberry PI without the compile hanging due to memory problems.
```
Open your /etc/dphys-swapfile (use sudo otherwise you get permission denied)
then edit the CONF_SWAPSIZE variable
```
CONF_SWAPSIZE=1024 <- from 100 to 1024

### Restart the swap service
To activate the new swap space
```
sudo /etc/init.d/dphys-swapfile stop
sudo /etc/init.d/dphys-swapfile start
```

### Compile OpenCV
```
sudo make -j4
```

### Install OpenCV 3 on your Raspberry Pi 3
```
sudo make install
sudo ldconfig
```