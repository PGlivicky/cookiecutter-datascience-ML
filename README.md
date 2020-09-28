# Cookiecutter Data Science ML

_A personalized fork of [Grip on Data Science](https://github.com/waveFrontSet/grip-on-data-science) template which is based on
[Cookiecutter Data Science](https://github.com/drivendata/cookiecutter-data-science)._

For more details, [consult the documentation](https://wavefrontset.github.io/grip-on-data-science/#getting-started) of the Grip on Data Science project.

## Prerequisities
-----------
 - Installed conda with a base environment containing
     - git
     - make
     - spyder (or other editor of choice)
     - python>=3.6
     - [cookiecutter python package](http://cookiecutter.readthedocs.org/en/latest/installation.html) >= 1.4.0

Cookiecutter can be installed by:
``` bash
$ conda config --add channels conda-forge
$ conda install cookiecutter
```

## How to start a new project

Open conda prompt in the parent folder of where your new project folder should
be.

### Create fresh cookiecutter template:
------------

``` bash
$ cookiecutter https://github.com/PGlivicky/cookiecutter-datascience-ML.git
```
    
And follow the instructions.
    
### Initiate git:

Navigate to your project folder and run

``` bash
$ git init
```

### Create new spyder project:

``` bash
$ spyder
```

Then in menu select `Projects->New Project` and set up the existing directory.
    
### Create a new `conda` environment:
------------
    
    make create_environment

### Overview over the next steps
------------

After activating the `conda` environment, you are all set up. Here are the next
steps:

- Define how to obtain the raw data of your project and update the `data/raw`
  target in the `Makefile` accordingly.
- Define generic processing and clean up transformations in 
  `{{ cookiecutter.module_name }}/data/generic_processing.py` to produce interim data.
- Define project specific transformations to obtain the final data set in 
  `{{ cookiecutter.module_name }}/features/build_features.py`.
- Edit `{{ cookiecutter.module_name }}/models/model_config.py` to decide what
  models you want to build and what the target value of the prediction will be.
  Issuing `make train` will automatically split your dataset into a train and a
  test set and then fit the models on the train set.
- Edit `{{ cookiecutter.module_name }}/models/metric_config.py` to decide what
  metrics you want to use to evaluate the model performance. Issuing `make
  evaluate` will evaluate the models using the defined metrics on the test set.


### Installing development requirements
------------

    pip install -r requirements.txt

### Running the tests
------------

    pytest tests
