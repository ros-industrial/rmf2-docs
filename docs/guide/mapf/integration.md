# Integration

1. Implement a subclass of `BaseRobotController` that defines how each agent's move action
   is executed.
   - A `SharedMemoryAgent` [example](https://github.com/ros-industrial/res_mapf/blob/main/res_mapf/packages/res_plan_execution/src/res_plan_execution/robot_controllers/shared_memory_agent_controller/agent_shmd_controller.py)
     is provided, which communicates commands and completion status by writing to a
     [SharedMemoryDict](https://github.com/luizalabs/shared-memory-dict).
2. Implement the `MAPFSolverABC` interface using a classical MAPF solver.
