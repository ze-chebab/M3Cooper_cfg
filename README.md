# M3Cooper_cfg
Configurating robots by installing Linux and ROS

## Installing Linux
Installation de ROS Noetic pour Ubuntu 20.04 (lien)
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
sudo apt update
sudo apt install ros-noetic-ros-base
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin_make
source devel/setup.bash
echo $ROS_PACKAGE_PATH
```
En exécutant la dernière ligne, on doit avoir 2 emplacements: un dans le dossier /opt et l'autre dans le dossier /home

## Configuration du RPI
Installation de ssh
```
sudo apt install openssh-server
sudo apt install sshguard
```

### Installing the joystick
Installation de la manette avec le driver xpadneo (pour éviter les problèmes ertm, cf. ze-chebab/M3Cooper)
```
sudo apt-get install dkms linux-headers-`uname -r`
sudo apt-get install git
cd /tmp
git clone https://github.com/atar-axis/xpadneo.git
cd xpadneo
sudo ./install.sh
sudo bluetoothctl
[bluetooth]# scan on
[bluetooth]# pair <MAC>
[bluetooth]# trust <MAC>
[bluetooth]# connect <MAC>
```

Installation de la centrale inertielle (IMU)
```
sudo nano /boot/config.txt
  dtparam=i2c_arm=on
  dtparam=i2c1=on
sudo nano /etc/modules
	i2c-bcm2708
	i2c-dev
sudo reboot
sudo apt-get install i2c-tools libi2c-dev
sudo i2cdetect -y 1
```
En exécutant la dernière ligne, on doit apercevoir 68. 
			
Installation du programme pour l'utilisation de la centrale inertielle (cf. ze-chebab/M3Cooper)
```
sudo apt-get install ros-noetic-rosserial-arduino
sudo apt-get install ros-noetic-rosserial
cd /usr/share/arduino/libraries 
sudo git clone https://github.com/chrisspen/i2cdevlib.git
cd /tmp
wget http://www.airspayce.com/mikem/bcm2835/bcm2835-1.68.tar.gz
tar zxvf bcm2835-1.68.tar.gz
```
