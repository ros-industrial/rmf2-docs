# Getting Started

This page takes you from a clean Ubuntu 22.04 or 24.04 machine to a running demo.

## Get the source

Clone the `rmf_industrial` repo to your home folder (use `release-v0.2.0rc1` branch as of 21 Oct 2026).

```sh
git clone git@github.com:Briancbn/rmf_industrial.git
cd ~/rmf_industrial

git switch --track -c release-v0.2.0rc1 origin/release-v0.2.0rc1
```

## Launch RMF-Industrial Core Services

Run the following commands to launch the RMF Industrial Core Services

```sh
cd ~/rmf_industrial
docker compose -f docker-compose.dev.yml up -d
```

This runs the essential communication broker for RMF Industrial Services
- RabbitMQ Broker
- Mosquitto MQTT Broker

And the following RMF2 Services
- VDA5050 Master
- Robot Execution System
  - Plan Server
  - Plan Executor
- Task Orchestrator
- Task Scheduler

### ⚠️ Port conflicts

The stack publishes the host ports below. If a previous run — or any other process (your own Postgres, Redis, RabbitMQ, Mosquitto, etc.) — is already holding one of them, startup will fail. Before starting, make sure they are free.

| Service           | Endpoint                                          |
| -------           | --------                                          |
| Task Scheduler    | `http://localhost:8080/task_scheduler`            |
| Task Orchestrator | `http://localhost:8080/task_orchestrator`         |
| Plan Server       | `http://localhost:8080/plan_server`               |
| Plan Executor     | `http://localhost:8080/plan_executor`             |
| VDA5050 Master    | `http://localhost:8080/vda5050_master`            |
| MQTT Broker       | `mqtt://localhost:1883`                           |
| AMQP Broker       | `amqp://localhost:5672`, `http://localhost:15672` |

You MUST ensure every port below is free before starting — each command should return
nothing. Stop or remove whatever shows up.

```sh
sudo lsof -nP -i :8080 -i :1883 -i :8000 -i :5672 -i :15672
```

## Download & Launch the Warehouse Simulation

Download and unpack the latest UE5 simulation build in your home folder:

```sh
curl -OL https://downloads.rmf-industrial.org/UE5Demos/RMF2_SIM_v0.2.0.zip

unzip RMF2_SIM_0.2.0.zip
```

Start the simulation

```sh
cd ~/RMF2_SIM_v0.2.0
./Linux/RMF2_SIM.sh
```

The simulation should start full screen matching your screen resolution.  
You can press `Alt + Enter` to toggle between fullscreen and windowed mode.  
You can also start the simulation in a windowed mode with a specific resolution.

```sh
./RMF2_SIM.sh -windowed -resx 1920 -resy 1080
```

The simulation controls are similar to a typical game.
Here are the basics

- Movement: Keyboard `W A S D` or Controller Left Stick
  - `W`, `S` move forward and backward
  - `A`, `D` pan left and right
  - `Q`, `E` move up and down

  View: Mouse  or Controller Right Stick
  - click on the screen and hold to move around

Other controls
- Tab `M` to show Map marker

## Troubleshooting

The stack runs its own brokers, so stop any system-wide ones that may conflict:

```sh
sudo systemctl stop mosquitto.service 2>/dev/null || true
sudo systemctl stop rabbitmq-server.service 2>/dev/null || true
```
::: tip
See [Simulation Troubleshooting](./simulation#troubleshooting) if the simulation doesn't run as expected. Some environments (e.g. newer Ubuntu releases or mixed integrated/discrete GPU machines) may need extra steps.
:::
