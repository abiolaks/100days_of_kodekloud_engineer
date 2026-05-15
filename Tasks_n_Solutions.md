## MIssion Critical Task
### Task 1
__Objectives__:
Setting up a virutal enviroment is one of the first step in building a machine learning learning project.

The xFusionCorp Industries data science team needs a standardised Python environment for their new ML project. Set up a virtual environment with the required ML libraries on the controlplane host.

- Create a Python virtual environment named ml-env under /root/code/ using python3 -m venv.

- Activate the environment and install the following packages: numpy, pandas, scikit-learn, and matplotlib.

- Generate a requirements.txt file using pip freeze and save it at /root/code/requirements.txt.

### Solution
python3 -m venv ml-env - virtual enviroment creation
source ml-env/bin/activate - activate the virtual environment
pip install numpy scikit-learn matplotlib pandas
pip freeze > requirements.txt


### Task 2
Fixing the correct configuration for the jupyterlab configuration settings

Jupyter configuration file for the xFusionCorp Industries data science team

--- xFusionCorp team overrides (review before starting the server) ---
c.ServerApp.token = ''
c.ServerApp.password = ''
c.ServerApp.disable_check_xsrf = True
c.ServerApp.notebook_dir = '/root/notebooks/'
c.ServerApp.port = 8888
c.ServerApp.ip = '0.0.0.0'


