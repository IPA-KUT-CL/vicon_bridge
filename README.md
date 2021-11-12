# VICON_BRIDGE
## Introduction
This is a driver providing data from VICON motion capture systems. It is based on the vicon_mocap package from the starmac stacks. Additionally, it can handle multiple subjects / segments and allows to calibrate an origin of the vehicle(s) as this is somehow tedious with the VICON Tracker.
- Markus Achtelik <markus.achtelik@mavt.ethz.ch>
- License: BSD, based on vicon_mocap from the starmac stacks
- Source: git https://github.com/ethz-asl/vicon_bridge.git (branch: master)
- Reference: http://wiki.ros.org/vicon_bridge
## Quick start
>>roslaunch vicon_bridge vicon.launch

## Important Parameters
1. datastream_hostport := IP_SERVER:PORT_SERVER. 

    The default server IP and port are `192.168.94.81:801`.

2. stream_mode := `ServerPush` or `ClientPull`. 
    
    **Note**: The developer recommend on `ClientPull` for there're problems with frame drop and buffering on mode `ServerPush`. However, the mode `ClientPull` add some delay. 
    
    TODO: check the delay

3. tf_ref_frame_id

    TF reference frame id.default = `world`.  


## Nodes

### 1. vicon_bridge  
interface to the Vicon DataStream SDK  
#### 1.1 published topics  
- vicon/<subject_name>/<segment_name>  
    pose of the subject/segment  
    msg: [geometry_msgs/TransformStamped](http://docs.ros.org/en/api/geometry_msgs/html/msg/TransformStamped.html)
- vicon/markers  
    publishes position of all labeled and unlabeled markers. Labeled markers are not affected by origin calibration.
    msg: [vicon_bridge/Markers](http://docs.ros.org/en/jade/api/vicon_bridge/html/msg/Markers.html)

#### 1.2 Services
- /calibrate_segment (vicon_bridge/viconCalibrateSegment)  
    service to calibrate the origin of a segment.
- /grab_vicon_pose (vicon_bridge/viconGrabPose)  
    grabs n poses for a given subject/segment and returns the averaged pose.

#### 1.3 Parameters
##### static parameters
- stream_mode (string, default: "ClientPull)  
    mode to connect to the DataStream server. Values: "ClientPull", "ServerPush".
- datastream_hostport (string, default: vicon:801)  
    host and port of the DataStream server (ip/hostname:port).
- tf_ref_frame_id (string, default: world)  
    tf reference frame id
##### origin calibration parameters
- /<subject_name>/<segment_name>/zero_pose/orientation/w (double, default: 1)
- /<subject_name>/<segment_name>/zero_pose/orientation/x (double, default: 0)
- /<subject_name>/<segment_name>/zero_pose/orientation/y (double, default: 0)
- /<subject_name>/<segment_name>/zero_pose/orientation/z (double, default: 0)
- /<subject_name>/<segment_name>/zero_pose/position/x (double, default: 0)
- /<subject_name>/<segment_name>/zero_pose/position/y (double, default: 0)
- /<subject_name>/<segment_name>/zero_pose/position/z (double, default: 0)

#### 1.4 TF transforms
- world -> vicon/<subject_name>/<segment_name>  
    tf transform from world to vicon/<subject_name>/<segment_name>.from can be changed with ~tf_ref_frame_id.


## TODO:
- [ ] explain subject and segment  
    defined in Tracker or Nexus, but how?

