#Eye in the Sky lab

This project attempted to get a Parrot ARdrone2 to automatically follow a user-defined target.  The drone's video feed is additionally made available on the user's (Android) smartphone.

A short video demonstrating this project can be found here:  https://www.youtube.com/watch?v=hAimGCS3YuY

#For Best Results:
- Use uncommon, high-contrast images for tracking.  Sports logos are quite good for this, as are signs and recycling bins.  Avoid dull or common colors, the tracker may latch on to someone else, veer away, and proceed to get lost.
- Keep the driving laptop within 15m or so of the drone.  We don't know the specific range of the drone's wifi, so the distance is subject to change.
- Use outdoors in low/no wind or indoors in an open space.  The drone is very likely to bounce off the walls if used in narrow rooms or hallways.
- Avoid fast or excessive rotation.  The algorithm will most likely lose its target if it is rotated 90 degrees.

#To Install:
Tracking software:

1. Install ROS.  This code was tested and working on Indigo, other distros may not work
2. Install ardrone_autonomy with apt-get install ros-[distro]-ardrone-autonomy
3. Install the IBVS software as per its instructions
4. export the DRONE\_STACK variable, pointing to your ardrone\_autonomy install.  It will most likely be /opt/ros/[distro]/share/ardrone_autonomy
5. If it has never been done, calibrate the camera as per the ROS camera\_calibration package.

Phone Integration:
- Assuming your phone is compatible, install the ROS Image Viewer app from the Android Play Store.

#To Run:
1. Connect phone and laptop to the drone's wireless network
2. Verify the laptop's IP address, it should be 192.168.1.2.  If it is anything else, export that address as ROS_HOSTNAME and run roscore manually.
3. Run parrot\_IBVSController\_launcher\_Release.sh from the IBVS package.  It can be found in the launchers directory.  This will start numerous terminal windows to run roscore and the various tracking nodes.  (if you ran roscore manually, it will still try and fail to start a new one.  This is perfectly alright)
4. Run the dummy.py script in dummy_node.  It's just an image topic remapper.
5. Run the ROS Image Viewer app, connecting to the laptop's IP address.  If it crashes silently and/or suddenly, make sure that the ImageCompressed message is coming through the 'usb\_cam/image\_raw/compressed' topic.

#To Track and Follow Objects:
This can also be found on the IBVS package page.

1. In the TLD GUI window, press F5 to update the current frame to whatever the camera is showing.  Once the target is well within view, draw a box around them and press Enter.
2. The drone control/video should be drawing a box around the target that follows them as they move.  If this does not happen, the tracker and control software are not communicating properly.  AS OF NOW we don't know what causes this, kill all of the processes except for roscore and launch everything again.
3. Assuming the tracking is working properly, have the target move around slowly to improve the TLD algorithm's ability to detect them.  This shouldn't take longer than 30sec or so.
4. Hit 'Toggle Learning' to turn learning mode off.  If this is not done, the drone is liable to overload and will refuse all commands for minutes at a time.
5. Click into the command terminal (it'll be open in the 2-tab terminal instance).  Specific commands can be found in the IBVS documentation.  All we care about is takeoff ('t') and begin autopilot ('o').  When 'o' is pressed, the drone's video should now list CONTROL[ON].  Press spacebar until the drone's lights turn green, then hit 't' for takeoff.
- To land normally, hit 'y'.  In an emergency, the drone can be force-stopped with spacebar, but this instantly stops the rotors and drops the drone out of the sky.  Use with caution.


NOTE:  The TLD GUI contains a strange buffer/logger program that constantly saves images to the user's drive in the 'logs' directory of the IBVS install.  It generates nearly 1GB per minute and can cause numerous errors if the drive fills, so be aware.

#Errors:
- The tracker occasionally fails to communicate with the control software.  We aren't sure what's causing this or how to fix it, but it seems like a race condition.
- If learning mode is not turned off, the algorithm may attempt to learn from outside data in the bounding box.  Certain common patterns (trees, buildings, etc) can overload the tracking software as it attempts to match against every tree, branch, and shrub in the camera's view.  This kills the drone (it repeats its last known command until no longer locked up).
- If the phone app crashes unexpectedly, make sure that you are connecting to the proper IP address and that the expected topic is getting images (see above).
- If the video from the app freezes, it means that someone else has connected to the drone's wifi and is attempting to view its video.  For whatever reason, only one connected phone can run ROS Image Viewer at a time.



