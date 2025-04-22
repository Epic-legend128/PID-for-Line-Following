# PID-for-Line-Following
Using Process Integral Derivative for Line Following

## Program Used
Because it is easier to test the code, I used a free robot simulation software called [Trik Studio](https://trikset.com/en/products/trik-studio) to make and test all the program in this repository instead of a real robot. It uses a simulated EV3 Mindstorms robot.

## Normal Line Follow
A normal line follow using a robot is quiet simple. We use 1 color sensor, if it sees black we only put power on the right wheel so that we do a slight left turn, otherwise, we do the opposite. This is a pretty straightforward way of doing a line follow, however, it is quite slow.

## PID
PID is the Process Integral Derivative, a way of automatically correcting an automated action used in robotics without the need of manual intervention. It works by using some kind of sensor to calculate the error each time the action happens. This error is a simple subtraction between the sensor value and the desired value that the sensor should show for the action to be optimal, $error=sensor1-best$. Then there are three types of calculations made that use the error to correct it, the Process, the Integral and the Derivative phase.

### Process
In the Process phase we just use the calculated error and multiply it by some constant, Kp, to make its effect stronger or weaker, $P=Kp*error$.

### Integral
In the Integral phase we just take the Integral of the error up until now, meaning that we add up all of the errors up until this specific moment, so we need a type of total error variable to be kept which is incremented at the end of each repeated action. This value is then multiplied by another constant, Ki, which strengthens or weakens this effect, $I=Ki*totalError$. This happens because every time we correct an error another error is produced in the next run, so the integral kind of makes up for this unfixed error.

### Derivative
In the Derivative phase we just calculate the derivative of the error at this point in time. This happens by just subtracting the current error by the previous error, which is then multiplied by another constant, Kd, $D=Kd*(error-lastError)$. This derivative tells us about the rate of change of the error, so if it is small there is no need for a big change, however, if it is big, then something went horribly wrong and needs to be fixed fast. That is the basic idea behind it.

## PID for Line Following
When it comes to line following the PID error is retrieved from the light intensity sensor which gives values close to 100 when it sees white and values close to 0 when it sees black. After this error goes through the PID then we add up the values produced from all three phases and we add them to the base speed of one wheel while subtracting them from the base speed of the other wheel.<br>
There are two types of line following to be used, one with a single sensor and one with two sensors.

### Single Sensor
In this case, we use one sensor either on the right or the left of the black line and our input is the reflected light intensity sensor's value, and our theoretical best value would be for the sensor to point in the middle between the black and white part of the line, so if we take an average of the two $mid=\frac{black+white}{2}$, this mid will be our optimal. Then following the standard PID procedure we can get a fast line following method using 1 sensor.

### Two Sensors
When using two sensors, one will be placed on the right side of the black line whereas the second one will be placed on the left. In this case, the theoretical best value will be when both sensors give the same reading, meaning that the error is the difference between the reading of the two sensors, $error=sensor1-sensor2$. This line following technique with two sensors instead of one is more reliable than the single line following and can handle greater speeds and more sudden turns.

## Testing Speeds
I wrote up a program for all of these methods in Trik Studio to see how fast they were on the same stage with the same base speed. The two line follows which used the PID method were both pretty fast and did about the same speeds, however, in longer tracks with lots of turns the 2 sensor line follow will do slightly better, and also the two sensor line follow allows for crossing interesections of black lines, whereas the single sensor one will not work and will choose to change line. The original line follow which used no PID took about 60% more time to complete all the tracks.

## PID with Gyroscope
Alternatively, if you want to just go straight with no line to help the robot, then you can utilise a gyroscope, which is calibrated at a specific direction and after that it gives you the angle in degrees of where the robot is currently facing from the calibrated direction. In this case, the error is calculated as $error=dir-gyroSensor$, where dir is the angle from the oriented angle you want to head at and gyroSensor is the value that the gyroscope provides. And then you apply the standard PID with this error and the robot should follow in a straight line.





