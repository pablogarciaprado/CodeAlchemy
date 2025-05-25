◀️ [Home](../README.md)

# Virtual Environments
A virtual environment is an isolated workspace for a Python (or Conda) project where you can install specific packages and dependencies without affecting other projects or the global environment. In short: they let each project have its own clean, independent setup.

**Purpose**:
- Avoids version conflicts between projects.
- Keeps dependencies organized and project-specific.
- Ensures reproducibility across systems and environments.

**Tools**:
- venv / virtualenv: Python’s built-in tools for creating virtual environments.
- conda: A package and environment manager that can create virtual environments supporting both Python and non-Python dependencies (e.g., R, C libraries), often used in data science.

## Conda Environments
### Create an environment

```bash
conda create --name myenv
conda create --name myenv python=3.9
```

### Delete an environment

```bash
conda env remove -n env_name
```

```bash
conda remove --name <env_name> --all
```

### Show the available environments

```bash
conda env list
```

```bash
conda info --envs 
```

### Show dependencies installed in the environment

```bash
conda list -n myenv
```

### Activate the environment

```bash
conda init # necessary to run it first if it's the first time
conda activate myenv
```

### Deactivate the environment

```bash
conda deactivate
```

### Locate base directory
Run the following command to locate the base directory where Conda is installed:
```bash
conda info | grep -i 'base environment’
```

Once you have the base directory, append `/etc/profile.d/conda.sh` to it.

## Install dependencies

- **using pip:**

```bash
pip install sheet-logger
pip install sheet-logger==1.0.7.1
pip show sheet-logger # verify the installation
```

- **from** `requirements.txt`

```bash
pip install -r requirements.txt
```

## Create an environment based on a `requirements.txt`

```bash
conda create --name myenv python=3.11
conda activate myenv
pip install -r requirements.txt
```

## **Generate a requirements.txt**

Run the following command to automatically list all the installed packages (and their versions) into a `requirements.txt` file:

```bash
pip freeze > requirements.txt
```

This will create a `requirements.txt` file in your current directory with all the installed packages and their respective versions.