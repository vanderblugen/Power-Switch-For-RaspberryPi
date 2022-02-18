# Power-Switch-For-RaspberryPi
This for adding a power switch to trigger a script to turn off or on a RaspberryPi
Information on this page was obtained from [here](https://scribles.net/adding-power-switch-on-raspberry-pi).
This page was created for my ease of access.  Use information from this page at your own discretion.   
  
## For this you need a RaspberryPi, momentary switch, and adequate wiring

# Wiring in
Attach the momentary switch via wiring to GPIO pin 5 and 6 on the RaspberryPi
[Link1 Reference](https://pinout.xyz/#), [Link2 Reference](https://pimylifeup.com/raspberry-pi-pinout/)

# Shutdown and reboot script
````bash
sudo nano /usr/local/bin/power-switch.py
````
````bash
import threading, subprocess
import RPi.GPIO as GPIO

if __name__ == '__main__':
    try:
        GPIO.setmode(GPIO.BOARD)
        GPIO.setup(5, GPIO.IN)
        GPIO.wait_for_edge(5, GPIO.RISING)
        pin = GPIO.wait_for_edge(5, GPIO.FALLING, timeout=3000)
        if pin is None:
            subprocess.call('sudo shutdown -h now', shell=True)
        else:
            subprocess.call('sudo reboot', shell=True)
    finally:
        GPIO.cleanup()
````
## What it does
<ul>
  <li>Push and hold for > 5 seconds shutdown the RaspberryPi</li>
  <li>Push and hold for < 5 seconds reboots the RaspberryPi</li>
</ul>

# Enable script at boot
````bash
sudo nano /etc/rc.local
````

Add a line before ````exit 0```` to execute the script at boot.

````bash
python /usr/local/bin/power-switch.py &
`````

# Done
Be sure to test that it is functional. 


![alt text](https://scribles.net/wp-content/uploads/2017/12/power-switch-768x338.png "Logo Title Text 1")
![alt text](https://i.postimg.cc/vHNB4pYG/1.jpg "Logo Title Text 1")
