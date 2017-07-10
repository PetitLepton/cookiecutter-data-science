{{cookiecutter.project_name}}
==============================

{{cookiecutter.description}}

==============================

First, add the data folders:
- go to the `{{cookiecutter.project_name}}` repository;
- run `make add_local_variables` to create an environment variable of the project directory;
- run `make data_folders` since, if you cloned the project, the folders do no exist;

Second, install `conda` and set up both the virtual environment and the Python packages:
- run `make download_conda` and follow the last instruction for installation;
- run `make create_environment` to create a `conda` environment named `{{cookiecutter.project_name}}`;
- load the environment by `source activate {{cookiecutter.project_name}}` and run `make install_requirements` to download the Python modules;
- run `jupyter kernelspec list` and check that the kernel points to the conda `{{cookiecutter.project_name}}` environment, not a local one (see [this page](https://jupyter-client.readthedocs.io/en/latest/kernels.html#kernelspecs)).

If you need to install new packages, please add them in the dependencies section from `{{cookiecutter.project_name}}.yml`.

==============================

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
    └── tox.ini            <- tox file with settings for running tox; see tox.testrun.org


--------

<p><small>Project based on the <a target="_blank" href="https://drivendata.github.io/cookiecutter-data-science/">cookiecutter data science project template</a>. #cookiecutterdatascience</small></p>
