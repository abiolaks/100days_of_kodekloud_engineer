## MIssion Critical Task
### Task 1
```__Objectives__:
Setting up a virutal enviroment is one of the first step in building a machine learning learning project.

The xFusionCorp Industries data science team needs a standardised Python environment for their new ML project. Set up a virtual environment with the required ML libraries on the controlplane host.

- Create a Python virtual environment named ml-env under /root/code/ using python3 -m venv.

- Activate the environment and install the following packages: numpy, pandas, scikit-learn, and matplotlib.

- Generate a requirements.txt file using pip freeze and save it at /root/code/requirements.txt.
```

### Solution
```
- virtual enviroment creation
python3 -m venv ml-env

- activate the virtual environment
source ml-env/bin/activate

- installing dependencies
pip install numpy scikit-learn matplotlib pandas
pip freeze > requirements.txt
```


### Task 2
```
Fixing the correct configuration for the jupyterlab configuration settings

Jupyter configuration file for the xFusionCorp Industries data science team

--- xFusionCorp team overrides (review before starting the server) ---
c.ServerApp.token = ''
c.ServerApp.password = ''
c.ServerApp.disable_check_xsrf = True
c.ServerApp.notebook_dir = '/root/notebooks/'
c.ServerApp.port = 8888
c.ServerApp.ip = '0.0.0.0'
```


### Task 3
``` Fixing the libraries in requirements.in for uv package manager
and compiling it to requirements/txt using this command

-libraries in the requirements.in
scikit-learn
mlflow
pandas
numpy
```
```
uv pip compile requirements.in -o requirements.txt
```

#### Task 4
The xFusionCorp Industries ML team enforces code quality with ruff and black on every pull request. The project at /root/code/fraud-detection/ currently fails both tools. Make it pass them.


The project at /root/code/fraud-detection/ contains a pyproject.toml and sample sources under src/.

The corrected project must meet the following requirements:

ruff and black are both configured with a line length of 120.
ruff lint rule selection includes E, F, W, and I, and is declared under [tool.ruff.lint] – The schema required by ruff 0.1 and later.
Running ruff check src/ from the project directory exits with status 0.
Running black --check src/ from the project directory exits with status 0.
Review the existing configuration and source files, and correct everything that prevents the two commands above from exiting cleanly.

ruff, black, and mypy are already installed.

#### Solution
- remove the unused import : import os
- modify the pyproject.toml file : set length for both black and ruff to 120, in the select add w and i to the errors
- final pyproject.toml should look like this-
```
    [project]
name = "fraud-detection"
version = "0.1.0"

[tool.ruff]
line-length = 120

[tool.ruff.lint]
select = ["E", "F", "W", "I"]

[tool.black]
line-length = 120
```

### Task 6
The xFusionCorp Industries deployment team needs the fraud-detection model code packaged as an installable Python distribution. A draft pyproject.toml exists at /root/code/fraud-detection/, but it does not build a wheel that meets the team's standard. Correct the file and produce a compliant package.


The project at /root/code/fraud-detection/ already contains the source code under src/fraud_detection/. The Python files are complete—you do not need to modify any of them.

The corrected pyproject.toml must satisfy every one of the following:

it declares a [build-system] section with requires = ["setuptools>=61.0", "wheel"] and build-backend = "setuptools.build_meta";
name is fraud_detection (the distribution name must match the module path under src/);
version is 0.1.0;
requires-python is >=3.10;
dependencies is ["scikit-learn", "pandas", "numpy"].
Review the existing pyproject.toml and correct everything that does not match the requirements above.

Build the package from the project directory:

   cd /root/code/fraud-detection
   python3 -m build

The build must produce a wheel named fraud_detection-0.1.0-*.whl under dist/.
The build package is already installed. Use python3 rather than python.

### Solution
- write this in the pyproject.toml file
```
[build-system]
requires = ["setuptools>=61.0", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "fraud_detection"
version = "0.1.0"
description = "Fraud detection model for xFusionCorp Industries"
requires-python = ">=3.10"
dependencies = ["scikit-learn", "pandas", "numpy"]

[tool.setuptools.packages.find]
where = ["src"]
```

### Task 7
The xFusionCorp Industries ML team enforces code quality on every commit via pre-commit. A draft .pre-commit-config.yaml exists in the git repository at /root/code/fraud-detection/, but it does not match the team's standard and pre-commit run --all-files fails against it. Correct the configuration.


A git repository already exists at /root/code/fraud-detection/ with .pre-commit-config.yaml and process.py already tracked. pre-commit is installed system-wide.

The corrected configuration must declare the following five hooks so that pre-commit run --all-files executes every one of them:

trailing-whitespace, end-of-file-fixer, and check-yaml – All three sourced from the pre-commit/pre-commit-hooks repository, pinned to a current release;
ruff – Sourced from the astral-sh/ruff-pre-commit repository, pinned to a current release;
black – Sourced from the psf/black-pre-commit-mirror repository, pinned to a current release.
Every repository entry in the configuration must include a rev: field.

Review the existing .pre-commit-config.yaml and correct everything that prevents the hooks above from running.

Once the configuration is correct, register the hooks with git and run them against the tracked files:

   pre-commit install
   pre-commit run --all-files

Tip: pre-commit autoupdate queries each referenced repository and rewrites the rev: pins to the latest released tag. This is the standard way to discover current versions without looking them up by hand.

### Solution
```
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v6.0.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml

  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.15.15
    hooks:
      - id: ruff

  - repo: https://github.com/psf/black-pre-commit-mirror
    rev: 26.5.1
    hooks:
      - id: black
```
- use:
- ```
  pre-commit autoupddate
  ```
  To queries each referenced and pins to the latest released tag

### Task 8
- The xFusionCorp Industries ML platform team maintains a Cookiecutter template that new ML projects are generated from.
- A draft template exists at /root/code/mlops-template/, but it does not render. Correct the template and use it to generate a project.

- A Cookiecutter template exists at /root/code/mlops-template/. cookiecutter is installed system-wide.
- The corrected template must satisfy every one of the following:
- The cookiecutter.json declares four variables:
- project_name (default my-ml-project)
- author (default xFusionCorp)
- python_version (default 3.11)
- ml_framework with the choices sklearn, pytorch, and tensorflow
- The generated requirements.txt logic:
- Contains scikit-learn when ml_framework is sklearn
- Contains torch when ml_framework is pytorch
- Contains tensorflow when ml_framework is tensorflow
- The generated README.md content:
- Must reference both the project_name and the author from cookiecutter variables.
- The template directory structure {{cookiecutter.project_name}}/ must contain:
- Files: README.md and requirements.txt
- Directories: data/, models/, src/, and tests/
- Review the existing template in the VS Code explorer and correct everything that prevents it from rendering.
- Once the template renders, generate a project at /root/code/churn-model/:
  ```
   cookiecutter /root/code/mlops-template/ -o /root/code/ --no-input project_name=churn-model ml_framework=sklearn
  ```
The generated project must contain a requirements.txt listing scikit-learn and a README.md that mentions xFusionCorp.
```
This is the what is in the requirement.txt

{% if cookiecutter.ml_framework = 'sklearn' %}
scikit-learn
{% elif cookiecutter.ml_framework = 'pytorch' %}
torch
{% elif cookiecutter.ml_framework = 'tensorflow' %}
tensorflow
{% if cookiecutter.ml_framework = 'sklearn' %}
scikit-learn
{% elif cookiecutter.ml_framework = 'pytorch' %}
torch
{% elif cookiecutter.ml_framework = 'tensorflow' %}
tensorflow
```
### Solution
```
The two bugs summarised
FileBugFixrequirements.txt= instead of == in Jinja2 conditionsChange all = to ==requirements.txtNo closing {% endif %}Add {% endif %} at the end
```

### Task 10
- The xFusionCorp Industries ML team is adopting DVC so that datasets and model files are versioned separately from code.
- Initialise DVC inside the existing Git repository at /root/code/fraud-detection/ and record the initialisation in Git.

- A Git repository already exists at /root/code/fraud-detection/ with an initial commit.

- Initialise DVC inside that repository so that the standard .dvc/ control directory and .dvcignore file are created alongside the existing Git working tree.

- Stage every file that DVC produces during initialisation, and record them in a new Git commit with the message Initialize DVC.

- Once initialisation is complete, the DVC extension will detect the new .dvc/ directory and surface the DVC TRACKED section in the EXPLORER panel together with - a DVC indicator in the bottom status bar.

### Solution
```
- change into the project folder and initialize dvc
dvc init

- stage the files created from dvc init using git
git add .

- commit the changes
git commit -m "initialize DVC"
```

### Task 11
- A teammate has added the transactions dataset to the xFusionCorp Industries fraud-detection repository, but it was committed directly to Git instead of being tracked with DVC. Bring the repository in line with the team standard—every dataset under data/ must be tracked by DVC, not by Git.


- A project exists at /root/code/fraud-detection/ with DVC already initialised. The dataset data/raw/transactions.csv is currently tracked by Git, and the team standard requires DVC to own it instead.

- Stop Git from tracking the dataset without deleting it from disk.

- Track the same dataset with DVC so a .dvc pointer file is produced and data/raw/.gitignore excludes the dataset itself.

- Stage the new .dvc pointer and the new .gitignore, then record a Git commit with the message Track transactions dataset with DVC.

- Once tracking is moved to DVC, the DVC TRACKED section in the EXPLORER panel will list the dataset, confirming the extension recognises it as a DVC-managed file.

### Solution
```
# remove the file from git tracking but not from the disk
git rm --cached data/raw/transactions.csv

# add the file to dvc for dvc to track
dvc add data/raw/transactions.csv

# Stage the file created from dvc add about using git add
git add data/raw/transaction.csv.dvc data/raw/.gitignore

# commit the stage changes from git
git commit -m "Transactions dataset track with DVC"

# check the dvc folder to see it has been trakc
```
### Task 12
```
- The xFusionCorp Industries ML team uses SeaweedFS as the shared S3-compatible object store for DVC-tracked data.
- A .dvc/config already declares a remote called s3 for the fraud-detection project, but dvc push currently fails. Correct the configuration and push the tracked data into the SeaweedFS bucket.

- A project exists at /root/code/fraud-detection/ with DVC initialised and data/raw/transactions.csv already tracked.
- SeaweedFS is already running on the controlplane:
- S3 endpoint: http://localhost:8333
- Filer UI: open the SeaweedFS Filer button at the top of the lab (forwarded port 8888) – buckets are visible under /buckets/.
- Credentials: weedadmin / weedadmin123 (already set in .dvc/config)
- Bucket name: dvc-storage (already created and visible in the Filer UI under /buckets/dvc-storage)
- Review the existing .dvc/config and correct everything that prevents dvc push from succeeding. The remote called s3 must:
- point at the dvc-storage bucket using s3://;
- use the correct SeaweedFS S3 endpoint URL;
- be marked as the default remote.
- Push the tracked data. After the push, the dvc-storage bucket in the SeaweedFS Filer UI must contain at least one object under the files/md5/... prefix.

```


### Solution
- update the .dvc.config file
```
[core]
    remote = s3
['remote "s3"']
    url = s3://dvc-storage
    endpointurl = http://localhost:8333
    access_key_id = weedadmin
    secret_access_key = weedadmin123
```
### Task 13
```
- A new xFusionCorp Industries team member has cloned the fraud-detection repository onto a fresh machine.
- The DVC remote is already configured to point at the team's SeaweedFS bucket, but dvc pull is failing. Diagnose the cause, correct the configuration, and pull the dataset.


- A cloned project exists at /root/code/fraud-detection/ with DVC initialised, the data/raw/transactions.csv.dvc pointer file present, but the dataset itself missing from disk and from the local DVC cache.

- SeaweedFS is already running on the controlplane and the dataset has already been pushed to the dvc-storage bucket—open the SeaweedFS Filer button at the top of the lab and navigate to /buckets/dvc-storage/ to confirm that the object is there.

- S3 endpoint: http://localhost:8333
- Credentials: weedadmin / weedadmin123
- Review .dvc/config and correct everything that prevents dvc pull from authenticating against SeaweedFS.

- After the fix, the s3 remote must use:
- The access key (access_key_id) weedadmin
- The secret key (secret_access_key) weedadmin123.
- Pull the dataset. After the pull, data/raw/transactions.csv must be present on disk and its content must match the hash recorded in the .dvc pointer.
```

### Solution
```
- enter this in the .dvc.config file
[core]
    remote = s3

['remote "s3"']
    url = s3://dvc-storage
    endpointurl = http://localhost:8333
    access_key_id = weedadmin
    secret_access_key = weedadmin123

- then do dvc pull
```
### Task 14
```
- The xFusionCorp Industries ML team uses DVC pipelines to keep data processing reproducible.
- A draft dvc.yaml exists in the fraud-detection project, but dvc repro does not complete the full pipeline. Correct the pipeline definition so it runs cleanly end to end.


- A project exists at /root/code/fraud-detection/ with DVC initialised.
- Python scripts are at src/data/process_data.py and src/data/split_data.py; raw input is at data/raw/transactions.csv.
- Do not modify the Python files or the input data.

- The corrected pipeline must declare two stages with the following behaviour:

- process_data – Depends on data/raw/transactions.csv and src/data/process_data.py; produces data/processed/clean_transactions.csv.
- split_data – Depends on data/processed/clean_transactions.csv and src/data/split_data.py; produces data/processed/train.csv and data/processed/test.csv.
- Review the existing dvc.yaml and correct everything that prevents dvc repro from completing.

- After your changes, dvc repro must run end to end and dvc status must report no stale stages.

-Once the pipeline is valid, the DVC extension's PIPELINES section under the DVC view will list both stages and visualise the dependency graph between them.
```
### Solution
```
- update the content of the dvc.yaml with this
stages:
  process_data:
    cmd: python src/data/process_data.py
    deps:
      - data/raw/transactions.csv
      - src/data/process_data.py
    outs:
      - data/processed/clean_transactions.csv

  split_data:
    cmd: python src/data/split_data.py
    deps:
      - data/processed/clean_transactions.csv
      - src/data/split_data.py
    outs:
      - data/processed/train.csv
      - data/processed/test.csv

- on the terminal run - dvc repro
- dvc status
if the first command run without error you are good"
```

