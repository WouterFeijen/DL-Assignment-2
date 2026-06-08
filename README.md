# [2AMM10] Deep Learning - Assignment 2

## Local Environment Setup
Similar to Assignmnent 1, this project uses [uv](https://github.com/astral-sh/uv) to manage extremely fast, reproducible virtual environments. Our dependencies are locked to **Python 3.13** to ensure cross-platform compatibility.

### 1. Install uv
If you haven't already, install the `uv` package manager globally. Open your terminal or command prompt and run:

    python -m pip install uv

### 2. Create the Virtual Environment
Navigate to this project's root directory in your terminal and create a virtual environment. We specify Python 3.13 to match our locked requirements:

    uv venv <PATH_TO_VENV_FOLDER>\<VENV_NAME> --python 3.13

*(Note: If you do not have Python 3.13 installed on your system, `uv` is smart enough to automatically download the correct background binaries for you!)*

### 3. Activate the Environment
You must activate the environment before installing packages or running the code.

**For Windows:**

    <PATH_TO_VENV_FOLDER>\<VENV_NAME>\Scripts\activate

**For Mac/Linux:**

    source <PATH_TO_VENV_FOLDER>/<VENV_NAME>/bin/activate

### 4. Install Dependencies
**⚠️ Hardware Note (Important):** This environment is explicitly configured to use PyTorch **Nightly** builds with **CUDA 13.2**. 

Because half our team is using RTX 50-series (Blackwell) laptops, the stable PyTorch releases do not yet support our hardware. The requirements.txt will automatically pull these specialized nightly versions, which are fully backward-compatible with older cards (like the RTX 3090 or Ada generation GPUs).

Because these massive nightly binaries live on PyTorch's private servers and not the main Python registry, a standard install command will fail to locate them. Navigate to this project's root directory in your terminal. With the environment active, you must run this specific sync command to point uv to the right servers and bypass strict dependency locks:

    uv pip sync requirements.txt --extra-index-url https://download.pytorch.org/whl/nightly/cu132 --index-strategy unsafe-best-match

*(Note 1: If the installation fails at the very end with an os error 32 regarding a locked font file like DejaVuSans.ttf, completely shut down any running Jupyter Notebooks or Python scripts and run the command again).*
*(Note 2: If your terminal says `uv` is not recognized, add `py -m` in front of the command on Windows, or `python -m` on Mac/Linux).*


### 5. Link to Jupyter
To make this environment available inside Jupyter Notebook/Lab, register it as a kernel by running the following command while the environment is still active:

    python -m ipykernel install --user --name=<VENV_NAME> --display-name "Python 3.13 (<VENV_NAME>)"

## Modifying the Environment (For Contributors)
If you need to add new packages (like `scikit-learn` or `pandas`), do not edit `requirements.txt` directly. Doing so will break the strict PyTorch Nightly locks.

Add your new package name to `requirements.in`.

Compile a new locked file using this command (the flags are required to allow pre-release builds and search the extra index):

    uv pip compile requirements.in -o requirements.txt --prerelease=allow --index-strategy unsafe-best-match

Run the sync command from Step 4 again to install your new packages and align your environment.

## Troubleshooting
**PyTorch using CPU instead of GPU?**
If your environment is set up correctly but your system silently defaults to the CPU, you likely need to update your desktop or laptop NVIDIA Display Drivers. While older GPUs (like the 30-series) are perfectly compatible with this setup, CUDA 13.2 requires the absolute latest Game Ready or Studio Drivers via the NVIDIA App / GeForce Experience to communicate with the hardware.