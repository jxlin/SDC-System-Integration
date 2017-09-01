## Udacity Self Driving Car Nanodegree Final Project: System Integration

This is Team Vulture project repo for the final project of the Udacity Self-Driving Car Nanodegree: Programming a Real Self-Driving Car. The project will require the use of Ubuntu Linux (the operating system of Carla) and a new simulator with integration with the Robotic Operation System or ROS.  This project will restrict the driving speed of Carla to ~ 10 MPH for field testing.  You may find our development and testing logs in the [REPORT.md](./REPORT.md) file.

### The Team

![Team Vulture Mascot](./imgs/vulture.JPG)

The following are the member of Team Vulture.

* __Team Lead__: John Chen, diyjac@gmail.com
* Rainer Bareiß, rainer_bareiss@gmx.de
* Sebastian Trick, sebastian.trick@gmail.com
* Yuesong Xie, cedric_xie@hotmail.com
* Kungfeng Chen, kunfengchen@live.com

__GO VULTURE!__

### Installation 

* Be sure that your workstation is running Ubuntu 16.04 Xenial Xerus or Ubuntu 14.04 Trusty Tahir. [Ubuntu downloads can be found here](https://www.ubuntu.com/download/desktop). 
* If using a Virtual Machine to install Ubuntu, use the following configuration as minimum:
  * 2 CPU
  * 2 GB system memory
  * 25 GB of free hard drive space

  The Udacity provided virtual machine has ROS and Dataspeed DBW already installed, so you can skip the next two steps if you are using this.

  __NOTE: *We experienced poor performance using VM, so did not use it for integration and testing.*__

* Follow these instructions to install ROS
  * [ROS Kinetic](http://wiki.ros.org/kinetic/Installation/Ubuntu) if you have Ubuntu 16.04.
  * [ROS Indigo](http://wiki.ros.org/indigo/Installation/Ubuntu) if you have Ubuntu 14.04.
  * [Dataspeed DBW](https://bitbucket.org/DataspeedInc/dbw_mkz_ros)
  * Use this option to install the SDK on a workstation that already has ROS installed: [One Line SDK Install (binary)](https://bitbucket.org/DataspeedInc/dbw_mkz_ros/src/81e63fcc335d7b64139d7482017d6a97b405e250/ROS_SETUP.md?fileviewer=file-view-default)

* Download the [Udacity Simulator](https://github.com/udacity/self-driving-car-sim/releases/tag/v0.1).

    __NOTE__: *If you are installing in native Ubuntu 16.04, the Dataspeed DBW One Line SDK binary install will auto install 4.4.0-92-generic Linux kernel, which will break CUDA and the NVIDIA 375 drivers if you have NVIDIA GPU in your native Ubuntu 16.04 build.  This will cause starting the simulator to fail because it can no longer open the OpenGL drivers provided by NVIDIA:*

    ![starting simulator failure image](./imgs/sim_startup_failure_caused_by_4.4.0-92-generic_kernel.png)

    *To fix this issue, you will have to:*
    * remove the 4.4.0-92-generic kernel (or any kernel newer than 4.4.0-91-generic):
        ```bash
        sudo apt-get remove linux-image-4.4.0-92-generic
        ```
    * reinstall the NVIDIA 375 drivers (follow the instructions):

        [https://askubuntu.com/questions/760934/graphics-issues-after-while-installing-ubuntu-16-04-16-10-with-nvidia-graphics](https://askubuntu.com/questions/760934/graphics-issues-after-while-installing-ubuntu-16-04-16-10-with-nvidia-graphics)
    
### Usage

1. Clone the project repository
```bash
git clone https://github.com/diyjac/SDC-System-Integration.git
cd SDC-System-Integration
```
2. __OPTIONAL__: Verify, List, Switch or Create your own branch in the repository
    * verify current branch
        ```bash
        git status
        ```
    * list existing branches
        ```bash
        git branch
        ```
    * switch to a different branch
        ```bash
        git checkout <different branch>
        ```
    * create new branch from current branch and push to remote repository
        ```bash
        git checkout -b <your branch>
        git push -u origin <your branch>
        ```
3. Install python dependencies
* With Pygame and MoviePy for Diagnostics and converting rosbags
    * For non-GPU Linux systems
        ```bash
        sudo -H pip install -r requirements-pygame.txt
        ```
    * For GPU Linux systems
        __NOTE__: Make sure to install CUDA and its dependencies!  [http://www.nvidia.com/object/gpu-accelerated-applications-tensorflow-installation.html](http://www.nvidia.com/object/gpu-accelerated-applications-tensorflow-installation.html)
        ```bash
        sudo -H pip install -r requirements-gpu-pygame.txt
        ```
* Without Pygame nor MoviePy
    * For non-GPU Linux systems
        ```bash
        pip install -r requirements.txt
        ```
    * For GPU Linux systems
        __NOTE__: Make sure to install CUDA and its dependencies!  [http://www.nvidia.com/object/gpu-accelerated-applications-tensorflow-installation.html](http://www.nvidia.com/object/gpu-accelerated-applications-tensorflow-installation.html)
        ```bash
        pip install -r requirements-gpu.txt
        ```
4. Make and run styx
```bash
cd ros
catkin_make
source devel/setup.bash
roslaunch launch/styx.launch
```
5. Run the simulator
```bash
unzip linux_sys_int.zip
cd linux_sys_int
chmod +x system_integration.x86_64
./system_integration.x86_64
```
6. To test grab a raw camera image
```bash
rosrun tools grabFrontCameraImage.py ../imgs/sampleout.jpg
```
![./imgs/sampleout.jpg](./imgs/sampleout.jpg)

7. To dump the waypoints from the `/base_waypoints` topic
```bash
rosrun tools dumpWaypoints.py ../data/simulator_waypoints.csv
```
8. To dump the final waypoints from the `/final_waypoints` topic
```bash
rosrun tools dumpFinalWaypoints.py ../data/final_waypoints.csv
```
![./imgs/sim_waypoint_map.png](./imgs/sim_waypoint_map.png)

9. To view the diagnostics screen in real-time when the integrated system is running
* __NOTE__: Requires pygame!
```bash
rosrun tools diagScreen.py --screensize 2 --maxhistory 800 --textspacing 75 --fontsize 1.5
```
![./imgs/sdc-t3-sysint-diag-screen.png](./imgs/sdc-sysint-diagnostics.gif)

10. To view sample Udacity provided rosbags, convert them to MP4, GIFS or JPG use the following:
* __NOTE__: Requires pygame and moviepy!
```
cd ../tools
python view_rosbag_video.py --dataset <rosbags>
python rosbag_video_2_mp4.py --dataset <rosbags> <path to mp4 file>
python rosbag_video_2_gif.py --dataset <rosbags> <path to gif file>
python rosbag_video_2_jpg.py --dataset <rosbags> '<path to rosbag_%04d.jpg>'
```
![./imgs/just_traffic_light.gif](./imgs/just_traffic_light.gif)
![./imgs/loop_with_traffic_light.gif](./imgs/loop_with_traffic_light.gif)

Full length MP4 videos of the Udacity provided sample rosbags are available for download:

* [./imgs/just_traffic_light.mp4](./imgs/just_traffic_light.mp4)
* [./imgs/loop_with_traffic_light.mp4](./imgs/loop_with_traffic_light.mp4)

Samples of jpeg images extracted:

![./test_images/loop_with_traffic_light_0283.jpg](./test_images/loop_with_traffic_light_0283.jpg)
![./test_images/just_traffic_light_0461.jpg](./test_images/just_traffic_light_0461.jpg)

CSV files with pose and manually updated labels:

* [./test_images/loop_with_traffic_light.csv](./test_images/loop_with_traffic_light.csv)
* [./test_images/just_traffic_light.csv](./test_images/just_traffic_light.csv)

SDC System Integration Carla Test Course Map from rosbag sample:

![./imgs/udacity-test-course-waypoint-map-from-rosbag.png](./imgs/udacity-test-course-waypoint-map-from-rosbag.png)

11. Sample training, validation and testing images have been collected and are in [data/collections/samples](./data/collections/samples) directory.  There is a [session1.csv](./data/collections/samples/session1.csv) file that will provide the features and labels.  To collect additional training, validation and testing images for the traffic light classifier, use the `autoTLDataCollector.py` tool once you have started `roslaunch launch/styx.launch` and the simulator:

```bash
cd SDC-System-Integration
mkdir data/collections/mysamples
cd ros
rosrun tools autoTLDataCollector.py mysamples/session1
```
![./imgs/sdc-sysint-auto-data-collector.gif](./imgs/sdc-sysint-auto-data-collector.gif)
