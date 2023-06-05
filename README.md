# FaceRecognitionRaspi3B-
Playing with my Raspberry Pi 3B+ using OpenCV

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
sudo apt-get install build-essential cmake pkg-config
sudo apt-get install libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev
sudo apt-get install libjpeg-dev libtiff5-dev
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
sudo apt-get install libxvidcore-dev libx264-dev
sudo apt-get install libatlas-base-dev gfortran
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
wget -O opencv.zip https://github.com/Itseez/opencv/archive/3.3.0.zip
unzip opencv.zip
```

### We also need to grab the opencv_contrib repository
```
wget -O opencv_contrib.zip https://github.com/Itseez/opencv_contrib/archive/3.3.0.zip
unzip opencv_contrib.zip
```

## Python installation
### Install pip
```
wget https://bootstrap.pypa.io/get-pip.py
sudo python get-pip.py
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
mkvirtualenv cv -p python3
```
