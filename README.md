# ai-car\

ROS(로봇 운영 시스템)에서 "노드"는 ROS 기반 로봇 시스템의 기본 구성 요소입니다. ROS는 로봇 소프트웨어 개발을 위한 도구와 라이브러리를 제공하는 미들웨어 프레임워크로, 노드는 ROS를 통해 통신하고 작업을 수행하는 개별 실행 가능한 프로그램 또는 프로세스입니다.

다음은 ROS 노드의 주요 특징 중 일부입니다:

독립성: 일반적으로 각 노드는 로봇의 모터 제어, 센서 데이터 처리 또는 고수준 계획과 같은 특정 작업을 수행하도록 설계됩니다. 노드는 상대적으로 독립적으로 작동하도록 설계되며, 시스템의 다른 부분에 작업을 분산하는 것이 쉽습니다.

통신: 노드는 주제를 게시하고 구독함으로써 서로 통신합니다. 주제는 노드 간 데이터를 교환하기 위한 명명된 통신 채널로, 노드는 주제에서 데이터를 교환합니다. 노드는 주제에 데이터를 게시할 수 있으며, 다른 노드는 해당 주제를 구독하여 데이터를 수신합니다.

메시지 전달: 노드는 ROS 특정 메시지 형식을 사용하여 메시지를 전달함으로써 통신합니다. 예를 들어, 노드는 주제에서 메시지로 센서 데이터를 게시할 수 있으며, 다른 노드는 해당 주제를 구독하여 데이터를 수신하고 처리할 수 있습니다.

비동기 작업: ROS 노드는 비동기적으로 작동하며, 서로 독립적으로 실행될 수 있습니다. 이는 로봇 시스템을 설계하고 실행하는 데 높은 병렬성과 유연성을 제공합니다.

명명: ROS의 각 노드는 ROS 그래프 내에서 고유한 이름을 가지고 있습니다. 이 이름은 ROS 네트워크에서 노드를 식별하고 주소 지정하는 데 사용됩니다.

시작: 노드는 일반적으로 ROS에서 제공하는 시작 파일 또는 명령행 도구를 사용하여 시작됩니다. 이러한 도구는 필요한 매개변수와 구성으로 여러 노드를 시작할 수 있습니다.

전반적으로 ROS 노드는 로봇 시스템을 설계하고 구현하기 위한 모듈식 및 분산 방식을 제공하며 복잡한 작업을 더 작고 관리 가능한 구성 요소로 분해하고 이러한 구성 요소 간의 효율적인 통신과 조정을 가능하게 합니다.-


5장, Teleop by keyboard
1.  teleop_keyboard 키 사용 방법
실행했을때 message입니다.
Control Your Robot!
---------------------------
Moving around:
        w
   a    s    d
        x

w/x : increase/decrease linear velocity (~ 1.2 m/s)
a/d : increase/decrease angular velocity (~ 1.8)

space key, s : force stop

CTRL-C to quit

2. 실행
# terminal #1
$ roslaunch jessicar_control keyboard_control.launch

# terminal #2
$ roslaunch jessicar_teleop jessicar_teleop_key.launch

1. gst-launch로 확인(Option)
camera orientation을 맞게 설정해야합니다. flip-method=0이 jetracer에게 맞는 방향입니다만 camera install에 따라 값을 0 또는 1로 변경해주세요.
gst-launch-1.0 nvarguscamerasrc sensor_id=0 ! 'video/x-raw(memory:NVMM),width=3280, height=2464, framerate=21/1, format=NV12' ! nvvidconv flip-method=0 ! 'video/x-raw,width=960, height=720' ! nvvidconv ! nvegltransform ! nveglglessink -e

2. csi_pub.py으로 확인
Jetson CSI camera를 image_view package를 통해 camera가 잘 동작하는지 확인합니다.
$ sudo apt install ros-melodic-image-view
​
# terminal #1
$ roslaunch jessicar_camera csicam.launch
# or
$ roslaunch jessicar_camera usbcam.launch

# terminal #2, PC or Jetson
$ rqt_image_view


3. Jetson streaming(Option)
Browser ← Jetson IP. PC 또는 폰 등에서 jetson에서 web으로 streaming하는 이미지를 확인할 수 있습니다.
먼저 Jetson에 아래 패키지를 설치해주세요.
$ sudo apt-get install ros-melodic-web-video-server
​
아래 두 명령을 먼저 실행해야합니다.
# terminal #1
$ roslaunch jessicar_camera cam_streaming.launch
