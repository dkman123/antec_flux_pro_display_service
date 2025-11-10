Install pre-requisites (I'm not sure if all of this is needed, but on a fresh install it won't hurt)
```
sudo apt install python3-pip python3-full python3-usb
python3 -m venv antec_display_venv
source antec_display_venv/bin/activate
pip3 install pyusb==1.3.1
```
Determine your sensors
```
# you NEED to edit the config file to determine the sensors it should read
# cd /sys/class/hwmon
# for each folder check the hardware sensor name:
# cat hwmon0/name
# look for motherboard vendor, or k10temp for AMD CPUs
# then check the temperature labels
# cat hwmon0/temp1_label
# set the sensor to the name file value, set the name to the label file value
```
Set the files in place and test it
```
# copy the config file
sudo mkdir /etc/antec
sudo cp sensors.conf /etc/antec/sensors.conf

# copy the program file
sudo cp antec_display_service.py /usr/bin/.

# to test run it
/usr/bin/python3 /usr/bin/antec_display_service.py
```
When that works you can deploy the service
```
# copy the service file to the correct folder (depending on distro this may be different)
sudo cp antec_display.service /usr/lib/systemd/system/.
# when you update service file you need to reload
sudo systemctl daemon-reload
# to start it
sudo systemctl start antec_display

# to make it automatically start with the system
sudo systemctl enable antec_display

# to check it's status
#sudo systemctl status antec_display
# to stop it
#sudo systemctl stop antec_display
```
