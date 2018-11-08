# arduino-vn100
Reading VectorNav VN100 IMU data through an Arduino as binary messages, dealing with checksum.

The provided code parses data sent from the VN100 in binary format. 
Below are the configurations required to be set on the VN100 to run the provided code without any alterations.
This assumes you are using an Arduino board which has more than one serial port. 
`Serial1` is connected to the IMU and `Serial` is connected to a computer through a USB cable. 
If you do not wish to visualize data in the Serial Monitor, you may use `Serial` as the IMU port.

## IMU Configuration
You can use the Serial Monitor to set following configurations on the IMU.
If you are using the Serial Monitor, make sure to change the line ending setting in the bottom of the Serial Monitor to `Newline`.
If you need any other parameters, you must change the group field values as described in the sensor manual.

```
$VNASY,0*XX                 // stop async message printing
$VNWRG,06,0*XX              // stop ASCII message outputs
$VNWRG,75,2,8,01,0128*XX    // output binary message (see below for details)
$VNCMD*XX                   // enter command mode
system save                 // save settings to flash memory
exit                        // exit command mode
$VNASY,1*XX                 // resume async message printing
```

## Configuring IMU Binary Message
Command | Register | Serial Port | Frequency | Group | Output Message | Checksum
------- | -------- | ----------- | --------- | ----- | -------------- | ---------
$VNWRG  | 75       | 2           | 8         | 01    | 0128           | *XX
Write to register command | Register number for the output binary message | This depends on the cable you use | divide 800 by this value get the required frequency, this example is for 100 Hz | Output group as defined in the manual | data categories from the group, this example is for b100101000 = 0x0128 | Use XX for unknown checksum values 
