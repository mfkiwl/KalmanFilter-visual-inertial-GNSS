# Visual-Inertial-GNSS-EKF
This is an extended Kalman filter for inertial-GNSS (currently) and visual-inertial-GNSS (finally) sensor fusion. The dynamic model now being used is 2-Dimensional, a bird's-eye view of the body inside world reference.<br /><br />
The filter will be tested on an agri-machinery for self-navigation and realising precise agriculture ([link](https://github.com/Jingxu-Li/Weed-Detector)). The hardware setup for the test includes a 6-DoF IMU (*Bosch BMI055/085*), an *Intel D455* depth camera, and a GNSS module (*NongXin AMG-PFZ202*). Updates on different dimensions, complexities,and working scenarios of the system may be added in the future.


# Log
**23/03/23** The first version of EKF for agri-machinery self-navigation, currently a framework. The next step is to tune the filter with IMU and GNSS module using [*realsense2*](https://github.com/IntelRealSense/librealsense) and [*pynmea2*](https://github.com/Knio/pynmea2).<br /><br />
**30/03/23** Updated funcs for fetching absolute positions and inertial sensor inputs from GNSS module and camera (Intel D455), the data was then sent to their buffers separately. Temporal sychronisation of the entire multi-sensor system was written but not complete. Current idea of TimeSync is to set firstly the GNSS samples as global temporal reference, then search IMU_accel, IMU_gyro, and Camera_rgb (ongoing) around that moment, and finally interpolates data for equivalent value and sent them all to the filter. The idea is similar to [*Stephen's 2012 paper*](https://ieeexplore.ieee.org/abstract/document/6696917).<br /><br />
**03/04/23** Initially finished TimeSync.py. Found some errors in coordinate descriptions and conversions since I have used a simple but wrong way (position x and position y) to design the whole functions. (Changes: 1. Moved body-to-world func from filterpipes.py to classOfuncs.py as a helper function; 2. Wrote something new to TimeSync.py; 3. Add a new Geo-to-Mtrs func to classOfuncs.py)<br /><br />
**04/04/23** Added functions to classOfuncs.py. Those were functions for converting coordinates or distances between geographical latitude-longitude representation and Cartesian x-y plane. Based on these new understandings, several changes should be made to filterpipes.py.<br /><br />
**10/04/23** Fixed several errors in former files. The inertial part of filterpipes.py was tested using a raw data from [*@smahajan07*](https://github.com/smahajan07/sensor-fusion). The visualised result shows the filter's availability at present, it may still be modified in future real-time tests. On the other hand, TimeSync.py waits to be debugged and tested using pseudo inputs.<br /><br />
**11/04/23** TimeSync.py debugged and tested. The module can be optimised by using ring buffers to store sensor inputs since the data before the synchronised point is useless for later computations.
