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
- Create the a directory data/raw (if it is not their or ceating the ML System for the first time)
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
        
- Pull the raw data from Google Drive (run `pip install dvc dvc-gdrive` if dvc command is not found)




``` shell
dvc pull
```