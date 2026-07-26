# Computer-Vision
All of the code required to master basic computer vision tracking with physical objects.

To use this code you will need an ESP-32, This will comtain both the python and C++ for you to learn Computer Vision Tracking and physical hardware

the code works by basically attaching a camera to a pan tilt mount and having the camera plug into the computer.

Important: some of the code wont work off the bat, in the python code you need to configure the following

SERIAL_PORT = "COM9"  
BAUD_RATE = 115200
CAMERA_INDEX = 0   

the serial com port is the number port your esp-32 is plugged into, the baud rate has to match the one on arduino ide or whatever software you use to flash your c++ code, the camera index is changed my changing the number to 0,1,2,3... based on how many integrated/external cameras you have if you have. if you have a standard and then the external you are using the external 90% of the time will be 0.

note: the c++ code contains all the pin configuration which you must change to match yours

all of the code in here representive my iterations of code and all have different purposes.
