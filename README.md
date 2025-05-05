# Isaac Lab Tutorial
This repository is all about Isaac Lab Tutorial for Imitation Learning.

# Installation

## Installation using Isaac Sim Binaries

Isaac Lab requires Isaac Sim. This tutorial installs Isaac Sim first from binaries, then Isaac Lab from source code.

### Installing Isaac Sim

Downloading pre-built binaries
Please follow the Isaac Sim documentation to install the latest Isaac Sim release.

From [Isaac Sim 4.5 release](https://docs.isaacsim.omniverse.nvidia.com/latest/installation/download.html), Isaac Sim binaries can be downloaded directly as a zip file. To check the minimum system requirements, refer to the documentation here.

We have tested Isaac Lab with Isaac Sim 4.5 release on Ubuntu 22.04 LTS with NVIDIA driver 535.129.

From Isaac Sim 4.5 release, Isaac Sim binaries can be downloaded directly as a zip file. The below steps assume the Isaac Sim folder was unzipped to the ${HOME}/isaacsim directory.

On Linux systems, Isaac Sim directory will be named ${HOME}/isaacsim.

Verifying the Isaac Sim installation
To avoid the overhead of finding and locating the Isaac Sim installation directory every time, we recommend exporting the following environment variables to your terminal for the remaining of the installation instructions:


 Linux
# Isaac Sim root directory
```
export ISAACSIM_PATH="${HOME}/isaacsim"
```
# Isaac Sim python executable
```
export ISAACSIM_PYTHON_EXE="${ISAACSIM_PATH}/python.sh"
```
For more information on common paths, please check the Isaac Sim documentation.

Check that the simulator runs as expected:


 Linux
# note: you can pass the argument "--help" to see all arguments possible.
```
${ISAACSIM_PATH}/isaac-sim.sh
```
Check that the simulator runs from a standalone python script:

# checks that python path is set correctly
${ISAACSIM_PYTHON_EXE} -c "print('Isaac Sim configuration is now complete.')"
# checks that Isaac Sim can be launched from python
```
${ISAACSIM_PYTHON_EXE} ${ISAACSIM_PATH}/standalone_examples/api/isaacsim.core.api/add_cubes.py
```
Caution

If you have been using a previous version of Isaac Sim, you need to run the following command for the first time after installation to remove all the old user data and cached variables:
```
${ISAACSIM_PATH}/isaac-sim.sh --reset-user
```
If the simulator does not run or crashes while following the above instructions, it means that something is incorrectly configured. To debug and troubleshoot, please check Isaac Sim documentation and the forums.

Installing Isaac Lab
Cloning Isaac Lab
Note

We recommend making a fork of the Isaac Lab repository to contribute to the project but this is not mandatory to use the framework. If you make a fork, please replace isaac-sim with your username in the following instructions.

Clone the Isaac Lab repository into your workspace:
```
git clone git@github.com:isaac-sim/IsaacLab.git
```
We provide a helper executable isaaclab.sh that provides utilities to manage extensions:
```
./isaaclab.sh --help
```
usage: isaaclab.sh [-h] [-i] [-f] [-p] [-s] [-t] [-o] [-v] [-d] [-n] [-c] -- Utility to manage Isaac Lab.

optional arguments:
   -h, --help           Display the help content.
   -i, --install [LIB]  Install the extensions inside Isaac Lab and learning frameworks (rl-games, rsl-rl, sb3, skrl) as extra dependencies. Default is 'all'.
   -f, --format         Run pre-commit to format the code and check lints.
   -p, --python         Run the python executable provided by Isaac Sim or virtual environment (if active).
   -s, --sim            Run the simulator executable (isaac-sim.sh) provided by Isaac Sim.
   -t, --test           Run all python unittest tests.
   -o, --docker         Run the docker container helper script (docker/container.sh).
   -v, --vscode         Generate the VSCode settings file from template.
   -d, --docs           Build the documentation from source using sphinx.
   -n, --new            Create a new external project or internal task from template.
   -c, --conda [NAME]   Create the conda environment for Isaac Lab. Default name is 'env_isaaclab'.

Creating the Isaac Sim Symbolic Link
Set up a symbolic link between the installed Isaac Sim root folder and _isaac_sim in the Isaac Lab directory. This makes it convenient to index the python modules and look for extensions shipped with Isaac Sim.

# Enter the cloned repository
```
cd IsaacLab
```
# create a symbolic link
```
ln -s path_to_isaac_sim _isaac_sim
```
# For example: ln -s ${HOME}/isaacsim _isaac_sim

Setting up the conda environment (optional)
Attention

This step is optional. If you are using the bundled python with Isaac Sim, you can skip this step.

If you use Conda, we recommend using Miniconda.

The executable isaaclab.sh automatically fetches the python bundled with Isaac Sim, using ./isaaclab.sh -p command (unless inside a virtual environment). This executable behaves like a python executable, and can be used to run any python script or module with the simulator. For more information, please refer to the documentation.

To install conda, please follow the instructions here. You can create the Isaac Lab environment using the following commands.

# Option 1: Default name for conda environment is 'env_isaaclab'
```
./isaaclab.sh --conda  # or "./isaaclab.sh -c"
```
# Option 2: Custom name for conda environment
```
./isaaclab.sh --conda my_env  # or "./isaaclab.sh -c my_env"
```
Once created, be sure to activate the environment before proceeding!

conda activate env_isaaclab  # or "conda activate my_env"
Once you are in the virtual environment, you do not need to use ./isaaclab.sh -p / isaaclab.bat -p to run python scripts. You can use the default python executable in your environment by running python or python3. However, for the rest of the documentation, we will assume that you are using ./isaaclab.sh -p / isaaclab.bat -p to run python scripts. This command is equivalent to running python or python3 in your virtual environment.

Installation
Install dependencies using apt (on Linux only):

# these dependency are needed by robomimic which is not available on Windows
sudo apt install cmake build-essential
Run the install command that iterates over all the extensions in source directory and installs them using pip (with --editable flag):
```
./isaaclab.sh --install # or "./isaaclab.sh -i"
```

By default, the above will install all the learning frameworks. If you want to install only a specific framework, you can pass the name of the framework as an argument. For example, to install only the rl_games framework, you can run:
```
./isaaclab.sh --install rl_games  # or "./isaaclab.sh -i rl_games"
```
The valid options are rl_games, rsl_rl, sb3, skrl, robomimic, none.

Attention

For 50 series GPUs, please use the latest PyTorch nightly build instead of PyTorch 2.5.1, which comes with Isaac Sim:


```
./isaaclab.sh -p -m pip install --upgrade --pre torch torchvision --index-url https://download.pytorch.org/whl/nightly/cu128
```

Verifying the Isaac Lab installation
To verify that the installation was successful, run the following command from the top of the repository:

# Option 1: Using the isaaclab.sh executable
# note: this works for both the bundled python and the virtual environment
```
./isaaclab.sh -p scripts/tutorials/00_sim/create_empty.py
```
# Option 2: Using python in your virtual environment
```
python scripts/tutorials/00_sim/create_empty.py
```
The above command should launch the simulator and display a window with a black viewport. You can exit the script by pressing Ctrl+C on your terminal. On Windows machines, please terminate the process from Command Prompt using `Ctrl+Break` or `Ctrl+fn+B`.

Simulator with a black window.
If you see this, then the installation was successful! 

## Train a robot:
You can now use Isaac Lab to train a robot through Reinforcement Learning! The quickest way to use Isaac Lab is through the predefined workflows using one of our Batteries-included robot tasks. 

Execute the following command to quickly train an ant to walk. Adding --headless is recommended for faster training:
```
./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/train.py --task=Isaac-Ant-v0 --headless
```
For a robot dog:
 ```
./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/train.py --task=Isaac-Velocity-Rough-Anymal-C-v0 --headless
```

# Resources
- Getting Started with Isaac Lab [[video]](https://www.youtube.com/watch?v=SjVqOqEXXrY)
- How to build Humanoid: NVIDIA Isaac Lab, how to walk [[video]](https://www.youtube.com/watch?v=xwOaStX0mxE)
- How to Train a Custom Quadruped Robot to Walk Using Isaac Lab [[video]](https://www.youtube.com/watch?v=z62oU4hM1xM)
- Isaac Sim 4.5 & Isaac Lab 2.0 | Installation & Overview [[video]](https://www.youtube.com/watch?v=CLFjtuH2NAQ)
- Creating an Empty Scene - Isaac Lab Tutorial 1 [[video]](https://www.youtube.com/watch?v=sL1wCfp9tRU)
- LycheeAI [[youtube]](https://www.youtube.com/@LycheeAI)
