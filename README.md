AIMLOps
==============================

AI MLOps Demo

Project Organization
------------

    ├── LICENSE
    ├── Makefile           <- Makefile with commands like `make data` or `make train`
    ├── README.md          <- The top-level README for developers using this project.
    ├── data
    │   ├── external       <- Data from third party sources.
    │   ├── interim        <- Intermediate data that has been transformed.
    │   ├── processed      <- The final, canonical data sets for modeling.
    │   └── raw            <- The original, immutable data dump.
    │
    ├── docs               <- A default Sphinx project; see sphinx-doc.org for details
    │
    ├── models             <- Trained and serialized models, model predictions, or model summaries
    │
    ├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
    │                         the creator's initials, and a short `-` delimited description, e.g.
    │                         `1.0-jqp-initial-data-exploration`.
    │
    ├── references         <- Data dictionaries, manuals, and all other explanatory materials.
    │
    ├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
    │   └── figures        <- Generated graphics and figures to be used in reporting
    │
    ├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
    │                         generated with `pip freeze > requirements.txt`
    │
    ├── setup.py           <- makes project pip installable (pip install -e .) so src can be imported
    ├── src                <- Source code for use in this project.
    │   ├── __init__.py    <- Makes src a Python module
    │   │
    │   ├── data           <- Scripts to download or generate data
    │   │   └── make_dataset.py
    │   │
    │   ├── features       <- Scripts to turn raw data into features for modeling
    │   │   └── build_features.py
    │   │
    │   ├── models         <- Scripts to train models and then use trained models to make
    │   │   │                 predictions
    │   │   ├── predict_model.py
    │   │   └── train_model.py
    │   │
    │   └── visualization  <- Scripts to create exploratory and results oriented visualizations
    │       └── visualize.py
    │
    └── tox.ini            <- tox file with settings for running tox; see tox.readthedocs.io


--------

<p><small>Project based on the <a target="_blank" href="https://drivendata.github.io/cookiecutter-data-science/">cookiecutter data science project template</a>. #cookiecutterdatascience</small></p>


# Setting up the Project

``` shell
ssh-keygen -t ed25519 -C "sashi@iisc.ac.in"
```
## Clone the repository
- Create a virtual environment using `venv` (you can also use `conda` instead of this)
``` shell
git clone https://github.com/StarsCDS/AIMLOps.git
```

## Install dependencies
- Create a virtual environment using `venv` (you can also use `conda` instead of this)
``` shell
make setup
source ~/.AIMLOps/bin/activate
python3 -m pip install --upgrade pip
```

- Install dependencies (you can also manually install all the dependencies from requirements.txt)
``` shell
make requirements
```

## Data Versioning with `dvc`
- Create a directory `data/raw` (if it is not their or ceating the ML System for the first time)
``` shell
mkdir  data/raw
```
- Copy the data file (e.g., data.csv) to the `raw` folder
``` shell
dvc init
dvc add data/raw
```
- To track the changes with git, run:
``` shell
 git add data/.gitignore data/raw.dvc
```
- To enable auto staging, run:
``` shell
dvc config core.autostage true
```
- Adding Gdrive as remote for storing and versioning the data
- Create a Folder over the Drive
- https://dvc.org/doc/user-guide/data-management/remote-storage/google-drive#authorization
- Pull the raw data from Google Drive (run `pip install dvc dvc-gdrive` if dvc command is not found)

``` shell
dvc remote add --default myremote gdrive://(ID is after https://drive.google.com/drive/folders/
dvc remote modify myremote gdrive_acknowledge_abuse true
```

## OR

- Adding AWS S3 as remote for storing and versioning the data
- Create s3 bucket in AWS. 
- It is advised to create a user with access to S3 (AmazonS3FullAccess or custum access to S3)
- Under the user, open `security and credentials` and create `Access keys` with CLI option copy ID and Key. You can add (optional) for future reference and for directly connecting S3 from your system.
``` shell
cd ~/
vi .credentials
```
``` 
add 
[default]
aws_access_key_id =  
aws_secret_access_key =  
```
- Now confiigure aws with dvc

``` shell
pip3 install dvc-s3
pip3 install awscli 
aws configure
dvc remote add --default myawss3 s3://(name of the bucket)
dvc push
```
## MLFlow using DagsHub
``` shell
touch src/models/Skln_Elasticnet.py
export MLFLOW_TRACKING_USERNAME= 
export MLFLOW_TRACKING_PASSWORD=
python3 Skln_ElasticNet.py
python3 Skln_ElasticNet.py 0.4 0.6
```

## Deploynment of containers to AWS
## Containerise FastAPI and test it with uvicorn webserver implementation
- Create API files, `__init__.py` and `main.py`, inside the folder `api`

- Add `Dockerfile` and configure 
``` shell
touch Dockerfile
``` 

- Use <a target="_blank" href="https://www.uvicorn.org"> uvicorn</a>, an ASGI web server implementation for Python.
- To start the server and check the API, use
``` shell
uvicorn api.main:app
```
# AWS ECR
- Create AWS ECR: `Create repositry`
- Allow access to IAM user: Permission-> ADD Permisions->Create inline permisions-> add ECR

## AWS setting for Github Action
- Under the repository, go to settings->security->Secrets and Variables->Actions-> Add `New Repository Key`
- `AWS_ACCESS_KEY_ID` `AWS_ACCOUNT_ID` `AWS_SECRET_ACCESS_KEY` `REPO_NAME`
- Under Actions add `New Workflow` -> set up a workflow yourself->`deploy.yml`
- Make sure to configure `aws-region:` (ECR region, e.g., ap-south-1) correctly
