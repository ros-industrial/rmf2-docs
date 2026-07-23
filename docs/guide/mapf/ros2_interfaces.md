# ROS 2 Interfaces

The two ROS 2 nodes communicate over the topics below. Robot-specific topics are namespaced
by `robot_id` (e.g. `/agent_0/plan`). Messages prefixed with `res_ros2_msgs/` are defined in
this repository (see [Messages](#ros2-messages)); `rmf_prototype_msgs/` types are from
[`next_gen_prototype`](https://github.com/open-rmf/next_gen_prototype).

## Plan Server Node

The `plan_server_node` solves the MAPF problem, generates plans, and tracks task status.

**Subscribes**

| Topic                           | Type                                       | Description                                                         |
| ------------------------------- | ------------------------------------------ | ------------------------------------------------------------------- |
| `/robot_onboard`                | `res_ros2_msgs/RobotOnboard`               | Registers a robot and its start location.                           |
| `/<robot_id>/task_request`      | `res_ros2_msgs/TaskRequest`                | A new destination for a robot; triggers (re)planning.               |
| `/<robot_id>/plan/progress`     | `rmf_prototype_msgs/Progress`              | Plan progress reported by the executor.                             |
| `/<robot_id>/plan/error`        | `rmf_prototype_msgs/PlanError`             | Errors encountered by the executor.                                 |
| `/committed_locations/response` | `res_ros2_msgs/CommittedLocationsResponse` | Locations currently committed to by agents, used during replanning. |
| `/destination/discovery`        | `rmf_prototype_msgs/ParticipantList`       | Discovered participants.                                            |

**Publishes**

| Topic                          | Type                                      | Description                                                   |
| ------------------------------ | ----------------------------------------- | ------------------------------------------------------------- |
| `/<robot_id>/plan`             | `rmf_prototype_msgs/Plan`                 | The plan a robot should execute.                              |
| `/committed_locations/request` | `res_ros2_msgs/CommittedLocationsRequest` | Requests the current committed locations before replanning.   |
| `/fleet/task_status`           | `res_ros2_msgs/TaskStatusUpdate`          | Task lifecycle updates (planning, in progress, completed, …). |

## Plan Executor Node

The `plan_executor_node` executes the plans on the robots and reports progress back to the server.

**Subscribes**

| Topic                          | Type                                      | Description                                     |
| ------------------------------ | ----------------------------------------- | ----------------------------------------------- |
| `/robot_onboard`               | `res_ros2_msgs/RobotOnboard`              | Registers a robot and its start location.       |
| `/<robot_id>/plan`             | `rmf_prototype_msgs/Plan`                 | The plan to execute for a robot.                |
| `/committed_locations/request` | `res_ros2_msgs/CommittedLocationsRequest` | Request for the executor's committed locations. |
| `/destination/discovery`       | `rmf_prototype_msgs/ParticipantList`      | Discovered participants.                        |

**Publishes**

| Topic                           | Type                                       | Description                                   |
| ------------------------------- | ------------------------------------------ | --------------------------------------------- |
| `/<robot_id>/plan/progress`     | `rmf_prototype_msgs/Progress`              | Plan progress as the plan is executed.        |
| `/<robot_id>/plan/error`        | `rmf_prototype_msgs/PlanError`             | Errors encountered during execution.          |
| `/committed_locations/response` | `res_ros2_msgs/CommittedLocationsResponse` | The executor's currently committed locations. |

## ROS2 Messages

Custom messages defined in [`res_ros2_msgs`](https://github.com/ros-industrial/res_mapf/tree/main/res_ros2_msgs/msg):

- **`RobotOnboard`** — `robot_id`, `start_location`. Registers a robot with the system.
- **`TaskRequest`** — `task_id`, `robot_id`, `goal`. Requests a robot to move to a destination.
- **`TaskStatus`** — `status` (enum: `PLANNING`, `PLANNED`, `SUPERSEDED`, `IN_PROGRESS`,
  `COMPLETED`, `AWAITING_REPLAN`, `PAUSED`, `EXECUTOR_PAUSED`, `FAILED`).
- **`TaskStatusUpdate`** — `task_id`, `robot_id`, `status` (`TaskStatus`), `source`
  (`"plan_server"` or `"plan_executor"`), `reason`, `superseded_by`, `timestamp`.
- **`CommittedLocation`** — `robot_id`, `location`, `task_id`, `waypoint_index`, `progress`.
  A location an agent has committed to.
- **`CommittedLocationsRequest`** — `request_id`. Asks executors to report committed locations.
- **`CommittedLocationsResponse`** — `request_id`, `committed_locations`
  (`CommittedLocation[]`), `stationary_agents`.
