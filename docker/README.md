## Install nvidia-docker
Follow instructions on this [link](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#installing-on-ubuntu-and-debian)

## Build

```bash
docker build -t urjc:foxy -f docker/Dockerfile .
```

## RUN

```bash
xhost +local:root
docker run --rm -it -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix:ro --gpus all urjc:foxy
```

```bash
docker run --rm -it -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix:ro --gpus all urjc:foxy ros2 launch pilot_urjc_bringup tb3_sim_launch.py
```

```bash
docker run --rm -it -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix:ro --gpus all urjc:foxy ros2 launch pilot_urjc_bringup nav2_turtlebot3_launch.py
```

```bash
docker run --rm -it -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix:ro --gpus all urjc:foxy ros2 launch mros2_reasoner launch_reasoner.launch.py
```

```bash
docker run --rm -it -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix:ro --gpus all urjc:foxy ros2 run mros2_reasoner mros2_publish_qa_node
```
