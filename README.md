# tortoisebot_3D_mapping
Tortoisebot 3D mapping is a package for the [tortoisebot](https://github.com/rigbetellabs/tortoisebot). This enables tortoisebot to map and navigate its environment in 3D using RTAB-Map.

### Launching RTAB-Mapping in Mapping mode `sim only`.

Run the following commands in the following order in separate terminals:
```bash
roslaunch tortoisebot_rtab_mapping tortoisebot_in_tb3_house.launch x_pos:=-3 y_pos:=1
roslaunch tortoisebot_firmware server_bringup.launch
roslaunch tortoisebot_navigation tortoisebot_sim_navigation.launch
roslaunch tortoisebot_rtab_mapping rtab.launch
```
### Launching RTAB-Mapping in Localisation mode `sim only`.

Run the following commands in the following order in separate terminals:
```bash
roslaunch tortoisebot_rtab_mapping tortoisebot_in_tb3_house.launch x_pos:=-3 y_pos:=1
roslaunch tortoisebot_firmware server_bringup.launch
roslaunch tortoisebot_navigation tortoisebot_sim_navigation.launch
roslaunch tortoisebot_rtab_mapping rtab.launch localization:=true
```

### To see error and the workaround solution.

**See Error:**
1. Run RTAB in mapping mode as is.
2. Send Nav goals and see the 3D map especially for points greater than 3m.
3. You'll see white planes and bad map (i.e. you'll see white layers instead of walls esp when wall is greater than 3m.)

*Alternatively:*
* Download [test.ply](https://drive.google.com/file/d/1dV8t5RMEbQa26zNdSdvzyOOYLAMIV5M2/view?usp=sharing) and view using `meshlab`.

**See Solution:** 
1.  Open `tortoisebot_3d_mapping/tortoisebot_rtab_mapping/gazebo/kinect_plugin.gazebo`
2.  Change line no `22` from `3` to `100`. ie:

*Before:*
```xml
<far>3</far>
```
*After:*
```xml
<far>100</far>
```
3. Send Nav goals and see the 3D map very clear and crisp.
4. You'll see white planes and bad map (i.e. you'll see white layers instead of walls esp when wall is greater than 3m.) will not be present and only objects will be shown.

*Alternatively:*
* Download [jugad.ply](https://drive.google.com/file/d/1mV_m3ejLXyHF_0ZpUmVV2sMGMDVJGk7x/view?usp=sharing) and view using `meshlab`.

### Theory:

My theory as to why this error is happening is that gazebo shows a white plane as infinity instead of showing `NaN`. This is why I think the problem is fixed when we set the range to 100. 

If you have a better solution to this feel free to correct this code and push the updates.

