cmake_minimum_required(VERSION 2.8.3)

project(test_rospy)
find_package(catkin REQUIRED COMPONENTS genmsg rosunit rostest std_msgs test_rosmaster)

if(CATKIN_ENABLE_TESTING)
  # messages are for integration testing only
  add_message_files(DIRECTORY msg
    FILES
    ArrayVal.msg
    EmbedTest.msg
    Empty.msg
    Floats.msg
    HeaderHeaderVal.msg
    HeaderVal.msg
    PythonKeyword.msg
    TestConstants.msg
    TestFixedArray.msg
    TransitiveImport.msg
    TransitiveMsg1.msg
    TransitiveMsg2.msg
    Val.msg
    NOINSTALL
    )
  add_service_files(DIRECTORY srv
    FILES
    ConstantsMultiplex.srv
    EmptyReqSrv.srv
    EmptyRespSrv.srv
    EmptySrv.srv
    ListReturn.srv
    MultipleAddTwoInts.srv
    StringString.srv
    TransitiveSrv.srv
    NOINSTALL
    )

  generate_messages(DEPENDENCIES std_msgs test_rosmaster)
endif()

catkin_package()

# Install scripts so shebangs are rewritten to match ROS_PYTHON_VERSION
catkin_install_python(
  PROGRAMS
    nodes/freq_talker
    nodes/listener.py
    nodes/listener_once.py
    nodes/listenerpublisher.py
    nodes/listenerpublisher_embed.py
    nodes/publish_on_shutdown.py
    nodes/talker
    nodes/talker.py
    test/rostest/add_two_ints_server
    test/rostest/fail_two_ints_server
    test/rostest/fake_time.py
    test/rostest/test_basic_services.py
    test/rostest/test_client_param_api.py
    test/rostest/test_client_param_server.py
    test/rostest/test_deregister.py
    test/rostest/test_embed_msg.py
    test/rostest/test_empty_service.py
    test/rostest/test_latch.py
    test/rostest/test_latch_unsubscribe.py
    test/rostest/test_node.py
    test/rostest/test_node_api.py
    test/rostest/test_on_shutdown.py
    test/rostest/test_pubsub_order.py
    test/rostest/test_rospy_client_online.py
    test/rostest/test_rospy_timer_online.py
    test/rostest/test_service_failure.py
    test/rostest/test_service_order.py
    test/rostest/test_sub_to_multiple_pubs.py
    test/rostest/test_topic_statistics.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

if(CATKIN_ENABLE_TESTING)
  catkin_add_nosetests(test/unit)

  # integration tests
  # - accidentally wrote two integration tests for client param API. They are different enough that it's no harm in having both
  add_rostest(test/rostest/client-param-server.test)
  add_rostest(test/rostest/client-param-api-2.test)
  add_rostest(test/rostest/services.test)
  add_rostest(test/rostest/deregister.test)
  add_rostest(test/rostest/pubsub-order.test)
  add_rostest(test/rostest/embed-msg.test)
  add_rostest(test/rostest/rospy.test)
  add_rostest(test/rostest/rospy_sim_time.test)
  add_rostest(test/rostest/node.test)
  add_rostest(test/rostest/latch.test)
  add_rostest(test/rostest/on_shutdown.test)
  add_rostest(test/rostest/sub_to_multiple_pubs.test)
  add_rostest(test/rostest/latch_unsubscribe.test)
  add_rostest(test/rostest/statistics.test)
endif()
