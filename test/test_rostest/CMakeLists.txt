cmake_minimum_required(VERSION 2.8.3)

project(test_rostest)

find_package(catkin REQUIRED COMPONENTS rostest)

catkin_package()

# Install scripts so shebangs are rewritten to match ROS_PYTHON_VERSION
catkin_install_python(
  PROGRAMS
    test/just_advertise.py
    test_nodes/talker.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

if(CATKIN_ENABLE_TESTING)
  add_rostest(test/publishtest.test)
endif()
