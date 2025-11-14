# 4. Jetson Nano Development Tutorial

<img class="common_img" src="../_static/media/section_4/media/image3.png" style="width:500px" />

## 4.1 Getting Started

### 4.1.1 Wiring Instruction

This section uses DuPont wires to connect ultrasonic sensor. For wiring instructions, refer to the figure below:

<img class="common_img" src="../_static/media/section_4/media/image4.png" style="width:700px" />

> [!NOTE]
>
> **Note: Before powering on, ensure that no metal objects are touching the controller. Otherwise, the exposed pins at the bottom of the board may cause a short circuit and damage the controller.**

### 4.1.2 Environment Configuration

Install NoMachine on your computer. The software package is located under "**[2 Software Tools & Programs -\> 01 Software Installation Package -\> Remote Desktop Connection Tool -\> 1 Remote Desktop Connection Tool](https://drive.google.com/drive/folders/1fPT8Rv4550TR0b_A6XEgt4Kk10eDRgGg?usp=sharing)"**. For detailed usage of NoMachine, refer to the materials in the same directory.

**Drag the program into the Jetson Nano system image, taking placing it on the desktop as an example.**

Open the terminal and enter the command to navigate to the program directory, enter:

```bash
cd Desktop/
```

## 4.2 Test Case

This example displays the status of the ultrasonic sensor detection in the terminal window.

### 4.2.1 Program Download

1. Open the terminal and enter the following command to navigate to the program directory:

```bash
cd Desktop/
```

2. Run the program by entering:

```bash
python3 UltrasonicSensorDemo.py
```

### 4.2.2 Project Outcome

Place obstacles at different distances in front of the ultrasonic sensor. The terminal will display the distances detected by the ultrasonic sensor.

<img class="common_img" src="../_static/media/section_4/media/image7.jpeg" style="width:550px" />

### 4.2.3 Program Brief Analysis

- **Import Libraries**

```py
import Jetson.GPIO as GPIO
import time
```

Import Raspberry Pi's GPIO library files and time library.

- **Initialization Operation**

```py
# Set GPIO pin numbers (设置GPIO针脚编号)
TRIG = 10
ECHO = 6

# Set GPIO mode to BCM (设置GPIO模式为BCM)
GPIO.setmode(GPIO.BCM)

# Set TRIG pin as output, ECHO pin as input (设置TRIG针脚为输出, ECHO针脚为输入)
GPIO.setup(TRIG, GPIO.OUT)
GPIO.setup(ECHO, GPIO.IN)
```

Set the corresponding GPIO pins, define the mode, and print the current status.

`TRIG = 10`: Set the TRIG pin number to 10.

`ECHO = 6`: Set the ECHO pin number to 6.

`GPIO.setmode(GPIO.BCM)`: Set the GPIO mode to BCM (Broadcom SOC channel numbering).

`GPIO.setup(TRIG, GPIO.OUT)`: Configure the TRIG pin as an output.

`GPIO.setup(ECHO, GPIO.IN)`: Configure the ECHO pin as an input.

- **Emit Ultrasonic Pulse**

```py
def distance_measurement():
    # Send ultrasonic pulse (发送超声波脉冲)
    GPIO.output(TRIG, True)
    time.sleep(0.00002)
    GPIO.output(TRIG, False)

    start_time = time.time()
    stop_time = time.time()

    # Record the time of ultrasonic emission and reception (记录超声波发射和接收的时间)
    while GPIO.input(ECHO) == 0:
        start_time = time.time()

    while GPIO.input(ECHO) == 1:
        stop_time = time.time()
```

To emit an ultrasonic pulse, set the TRIG pin to high level, wait for a short period, and then set it back to low level. Record the time of sending and receiving the ultrasonic wave using variables. When the ECHO pin receives the reflected signal, stop the timing.

- **Calculate Distance**

```py
# Calculate distance (计算距离)
    elapsed_time = stop_time - start_time
    distance = (elapsed_time * 34300) / 2  # 34300 is the speed of sound, unit is cm/s (34300为声速，单位是厘米/秒)
```

Calculate the round-trip time of the ultrasonic wave using the function, and multiply it by half the speed of sound (34300 cm/s) to obtain the distance.

- **Print data**

```py
try:
    while True:
        dist = distance_measurement()
        print(f"Distance: {dist:.2f}centimeter")
        time.sleep(0.1)
```

Call the distance_measurement function to obtain the measured distance, then print the value with two decimal places.