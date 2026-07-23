# Usage

Each step below runs in its own terminal, started from the workspace root. Steps that use
the built workspace and the ROS 2 nodes first need the **project environment**, which sources
the built workspace and activates its virtual environment:

```bash
cd ~/res_ws
source install/setup.sh
source install/activate.sh
```

## Local demonstration

**1. Run the PyBullet simulation.** Start it with the agents' start coordinates:

```bash
res_pybullet_sim --coords "0,0 2,0"
```

**2. Run the demonstration script.** In the _project environment_, run the integration demo:

```bash
python3 src/res_mapf/res_ros2/res_ros2/test/integration.py
```

## Demonstration with ROS 2 nodes

**1. Run the PyBullet simulation.**

```bash
res_pybullet_sim --coords "0,0 2,0"
```

**2. Run the ROS 2 plan server.** In a _project environment_, launch the plan server node:

```bash
ros2 run res_ros2 plan_server_node
```

**3. Run the ROS 2 plan executor.** In another _project environment_, launch the plan executor
node:

```bash
ros2 run res_ros2 plan_executor_node
```

**4. Onboard the robots**

```bash
ros2 topic pub -1 /robot_onboard res_ros2_msgs/RobotOnboard "robot_id: 'agent_0'
start_location: '0,0'"

ros2 topic pub -1 /robot_onboard res_ros2_msgs/RobotOnboard "robot_id: 'agent_1'
start_location: '2,0'"
```

**5. Send the initial tasks together**

```bash
ros2 topic pub -1 /agent_0/task_request res_ros2_msgs/TaskRequest "task_id: 'agent_0_task'
robot_id: 'agent_0'
goal: '2,0'" &

ros2 topic pub -1 /agent_1/task_request res_ros2_msgs/TaskRequest "task_id: 'agent_1_task'
robot_id: 'agent_1'
goal: '0,0'"
```

**6. Replace the tasks with new ones to trigger replanning**

```bash
ros2 topic pub -1 /agent_0/task_request res_ros2_msgs/TaskRequest "task_id: 'agent_0_task'
robot_id: 'agent_0'
goal: '1,2'" &

ros2 topic pub -1 /agent_1/task_request res_ros2_msgs/TaskRequest "task_id: 'agent_1_task'
robot_id: 'agent_1'
goal: '0,3'"
```
