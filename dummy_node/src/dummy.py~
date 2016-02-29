#!/usr/bin/env python

import roslib
import rospy

from sensor_msgs.msg import CompressedImage

def callback(data):
    pub.publish(data)

if __name__ == '__main__':
    rospy.init_node('dummy')
    pub = rospy.Publisher('usb_cam/image_raw/compressed', CompressedImage, queue_size=0)
    sub = rospy.Subscriber('drone1/camera/front/image_raw/compressed', CompressedImage, callback)
#subscribed topic should be correct now

    rate = rospy.Rate(10)
    while not rospy.is_shutdown():
        rate.sleep()




