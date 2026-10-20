# Getting Started

This page takes you from a clean Ubuntu 22.04 / ROS 2 Humble machine to a running demo.

## Get the source

Make a `ros_industrial_ws` directory in your home folder and clone the `rmf_industrial` repo (use `throwaway/x2-demo` branch as of 20 Oct 2026).

```bash
mkdir -p ~/ros_industrial_ws
cd ~/ros_industrial_ws

git clone git@github.com:Briancbn/rmf_industrial.git
cd ~/ros_industrial_wsrmf_industrial

git switch --track -c throwaway/x2-demo origin/throwaway/x2-demo

```

## Build the image

```bash
cd ~/ros_industrial_wsrmf_industrial
docker compose -f docker-compose.dev.yml up -d
```

> **TIP:** First-time builds are slow.

## Download & Launch the warehouse simulation

Download and unpack the latest UE5 simulation build:

```bash
cd ~/ros_industrial_ws
curl -OL https://downloads.rmf-industrial.org/UE5Demos/RMF2_SIM_v0.2.0.zip

unzip RMF2_SIM_0.2.0.zip
```

Start the simulation

```sh
cd ~/ros_industrial_ws/RMF2_SIM_v0.2.0/Linux
./RMF2_SIM.sh
```

The simulation should start full screen matching your screen resolution.  
You can press `Alt + Enter` to toggle between fullscreen and windowed mode.

You can also start the simulation in a windowed mode with specific resolution

```sh
./RMF2_SIM.sh -windowed -resx 1920 -resy 1080
```

The simulation controls are similar to a typical game.
Here are the basics

- Movement: Keyboard `W A S D` or Controller Left Stick
  - `W`, `S` move forward and backward
  - `A`, `D` pan left and right
  - `Q`, `E` move up and down
