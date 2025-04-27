# Firmware

## Summary
This folder contains the implementation for two relevant firmware components of the project. For one, our project needs to control the audio system so that users can hear translations from the speaker. Secondly, we have the code for implementing button control over the volume and mode toggling. Althought the relevant code already exists in everything.py, we still want to highlight the firmware components seperately in its own section. 

### Volume Control (volume_control.py)
volume_control.py takes advantage of os calls. Specifically it uses amixer to control increases and decreases in the volume using the functions calls to increase_volume() and decrease_volume. The script also runs an example under adjust_volume() to test modifying the volume.

### Button Control (button_control.py)
button_control.py using the gpiozero library to implement hardware interrupts. The buttons are pulled high via an internal pull up resistor. The other end of the buttons are connected to ground such that the software waits for the button to be pressed for the state to be pulled down to ground. Specifically, this script sets up three hardware interrupts using the following portion of code below. These lines of code setup the hardware interrupt parameters and attach a function that happens on event trigger.

``` python
button_mode = Button(PIN_1, pull_up=True, bounce_time=0.2)
button_up = Button(PIN_2, pull_up=True, bounce_time=0.2)
button_down = Button(PIN_3, pull_up=True, bounce_time=0.2)

button_mode.when_pressed = change_mode
button_up.when_pressed = volume_up
button_down.when_pressed = volume_down
```
