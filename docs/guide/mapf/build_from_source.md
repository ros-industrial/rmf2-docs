# Build From Source

## Setup with uv

**1. Install uv.** Install the [uv](https://docs.astral.sh/uv/) Python package manager:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**2. Build the colcon plugins required for uv support.** The packages in this repository are standard Python
projects, not ROS 2 ament packages. To allow `colcon` to build them and manage their dependencies with `uv`, install the
[`colcon-python-project`](https://github.com/colcon/colcon-python-project) and
[`colcon-python-project-uv`](https://github.com/Briancbn/colcon-python-project-uv) plugins
in a separate workspace:

```bash
mkdir -p ~/colcon_extra_ws/src
cd ~/colcon_extra_ws
git clone https://github.com/colcon/colcon-python-project.git -b devel src/colcon-python-project
git clone https://github.com/Briancbn/colcon-python-project-uv.git -b main src/colcon-python-project-uv
colcon build
. install/local_setup.sh
```

Sourcing `install/local_setup.sh` makes the plugins (including the `colcon venv` command)
available to `colcon` in the current shell.

## Build and Install

**3. Clone the sources.** Set up the project workspace and clone this repository together
with its dependency, [`next_gen_prototype`](https://github.com/open-rmf/next_gen_prototype):

```bash
mkdir -p ~/res_ws/src
cd ~/res_ws
git clone https://github.com/ros-industrial/res_mapf.git src/res_mapf
git clone https://github.com/open-rmf/next_gen_prototype.git src/next_gen_prototype
```

**4. Install system dependencies.** Use rosdep to install any remaining dependencies
declared by the packages:

```bash
rosdep install --from-paths src -y --ignore-src
```

**5. Build the project.** First source the colcon uv support so the `colcon venv` command is
available. `colcon venv sync` then creates a shared uv virtual environment for the workspace
and installs every package's dependencies into it (resolved from their `pyproject.toml`
files), so the subsequent `colcon build` has everything it needs:

```bash
source ~/colcon_extra_ws/install/setup.sh
colcon venv sync
colcon build
```

---
