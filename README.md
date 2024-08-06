# Access PCB via ssh

## in vscode download the following extensions 

**Remote - SSH**

**Remote - SSH: Editing Configuration Files**

**Remote Explorer**

Once the extensions are downloaded, in order to access ssh you must first make sure that both devices being used for ssh are on the same WIFI, for example, a laptop and a Jetson Orin Nano, if not remote access will not work. 

## For ssh you need the ip address of the device you are trying to access, t0 get it open the terminal and type:

`ifconfig`

This will show a lot of information and address so look for the words inet and then an example ip address would look like 192.168.10.5

**inet 192.168.10.5**

Now that both devices are on the same WIFI or connection you can now go in the terminal and type:

`ssh username@ip_address`

An example of this would be:

`ssh orin@192.168.10.5`

Enter the password and now you have remote access into the required device. To access vscode you can open vscode and on the bottom left corner of the vscode window you will see a green connection sign, so click it, and select **connect to host** and then enter:

`ssh username@ip_address`

Now you can use vscode remotely.



# Understanding the PCB for the Jetson Orin Nano

On the Jetson Orin Nano you will see Pins that are labeled, and these pins are very **important** to get the PCB to function correctly.

**Pin 1: PWR 5V**
**Pin 3: SDA (Data)**
**Pin 5: SCL (Time)**
**Pin 25: GND**

When connecting SDA and SCl make sure that **SDA is connected to SDA and SCL is connected to SCL**, because if not then you will fry you device 

**bus 1: Pins 3 and 5**
**bus 0: Pins 27 and 28**

Now that we understand what the required pins do and which pins are connected to each bus you can now check if the PCB works with the Jetson Orin Nano by connecting those pins from the PCB to the Jetson Orin Nano Pins.

 **PCB and Jetson Pins are the exact same spots** 

Once connected properly give power to the Jetson Orin Nano and a light should turn on the PCB indicating that connections are correct, if not double check Pins 3 and 5 to make sure they are connected correctly. 

Now we want to test that the PCB works, specifically the PCA9685 module, to do this we need to test some code using an analog motor. Follow the steps below to install and run the code. 

`git clone https://github.com/JetsonHacksNano/ServoKit.git`
`cd ServoKit/`
`clear`

## We will now check to see if we can see the device.

`ls -l /dev/i2c*`
**You will see all the different buses in the device**

## Now we need to check if we are apart of any groups so enter:

`groups`

## More than likely you will not be apart of any groups so we need to add this

`sudo i2cdetect -y -r 1`

You will notice that bus 1 will have these symbols "UU" this just means that the Jetson Orin Nano is currently using these addresses for its own drivers. Bus 1 is equivalent to bus 7 to type in:

`i2cdetect -y -r 7`

If everything was done correctly then you should see a 0x40 address pop up as the default on bus 7, if not and the grid is blank meaning no address are found, then we need to check the  PCA9685 component to see if it was fried, as not seeing the addresses is a good indication that the component needs to be switched out and replaced. 

Now that we have the correct addresses to use the PCB we will install python3 and use a simple Servo test code that will allow the user to see if everything is functioning as it should. 

We need to do some installations for the Servo kit so type in:

`./installServoKit.sh`

Now we need to reboot the Jetson Orin Nano so first log out of the device using:

`logout' or 'exit`

Then unplug the device for 10 seconds and plug it back in and enter via ssh again as previous steps have mentioned up above. 

Now we want to check what groups we belong to so type in the terminal: 

`groups`

If done correctly you should see groups i2c and gpio added



# Running the demo code via python3

`python3`

`from adafruit_servokit import ServoKit`

`kit = ServoKit(channels=16)`

Now to test the code type in:

`kit.servo[0].angle=130`

Change the angle for further testing for example 

`kit.servo[0].angle=90`
`kit.servo[0].angle=170`
`kit.servo[0].angle=40`

Once finished type in:

`quit()`
