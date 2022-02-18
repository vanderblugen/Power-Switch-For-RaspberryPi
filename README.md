# Power Switch For A RaspberryPi
This is for adding a power switch on a RaspberryPi to trigger a script to turn off or reboot.<br>
Information on this page was obtained from [here](https://scribles.net/adding-power-switch-on-raspberry-pi).<br>
This page was created for my ease of access.<br>
Use information from this page at your own discretion.<br>

## What is needed
* RaspberryPi
* Momentary switch
* Adequate wiring

## Wiring in
Attach the momentary switch via wiring to GPIO pin 5 and 6 on the RaspberryPi
Here's a few references
* [Reference 1](https://pinout.xyz/#)
* [Reference 2](https://pimylifeup.com/raspberry-pi-pinout/)

## Shutdown and reboot script
```bash
sudo nano /usr/local/bin/power-switch.py
```
```bash
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
```
What this does
* Push and hold for > 5 seconds shutdown the RaspberryPi
* Push and hold for < 5 seconds reboots the RaspberryPi

## Enable script at boot
```bash
sudo nano /etc/rc.local
```

Add a line before ````exit 0```` to execute the script at boot.

```bash
python /usr/local/bin/power-switch.py &
````

## Test
Be sure to test that it is functional. 

![Example 1](https://i.postimg.cc/502dq2gT/1.jpg)
![Example 2](https://scribles.net/wp-content/uploads/2017/12/power-switch-768x338.png)

