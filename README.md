# Cookiecutter Data Science ML

_A personalized fork of [Grip on Data Science](https://github.com/waveFrontSet/grip-on-data-science) template which is based on
[Cookiecutter Data Science](https://github.com/drivendata/cookiecutter-data-science)._

For more details, [consult the documentation](https://wavefrontset.github.io/grip-on-data-science/#getting-started) of the Grip on Data Science project.

## Prerequisities

To create and set up a new project based on this template you will need the following:

Required:
 - [conda](https://docs.conda.io/en/latest/miniconda.html#) >= 4.4
 - [cookiecutter python package](http://cookiecutter.readthedocs.org/en/latest/installation.html) >= 1.4.0

Cookiecutter can be installed from conda:
``` bash
> conda config --add channels conda-forge
> conda install cookiecutter
```

Recommended:
 - make (can be installed from conda: `conda install make`) is used one time to automatically create the new project's 
 conda environment (this can be done also manually, however); the created project's environment then also contains make 
 as dev-dependency by default for convenience.
 - [PyCharm community edition](https://www.jetbrains.com/pycharm/download/) or other IDE of choice (the detailed instructions
 here assume you use PyCharm, but a similar set up can be done in most IDEs)

## How to start a new project

### Create a fresh project from the cookiecutter

Open a conda prompt or another shell from which you can run cookiecutter (verify that you can by trying `cookiecutter --version`). 
Navigate to the parent folder of where your new project folder should
be and run

``` bash
> cookiecutter https://github.com/PGlivicky/cookiecutter-datascience-ML.git
```
    
You will be asked for the new project's name and some basic settings.

**Voila, your new project's folder has been set up for you!** 

You can explore it a bit and see a brief explanation of the created file structure 
in `<your new project's folder>/README.md`.

### Create and setup a new conda environment for your project

**If you have make installed:**

Open a conda prompt or another shell from which you can run make (verify that you can by trying `make --version`).
Navigate to `<your new project's folder>` and run

``` bash    
> make create_environment
```

**If you do not (yet) have make:**

First check the name you chose for your environment when setting up your project:
 - by default this is the same as the name of your new project's folder,
 - it is always configured in `<your new project's folder>/environment.yml` under the key 'name'.

This name is refered to as `<your conda environment name>` in the commands below.

Open a conda prompt and run

``` bash    
> conda env create --name <your conda environment name> -f environment.yml
> conda env export --name <your conda environment name> | grep -v "prefix:" > environment.lock
```

Either of the two options above will create a new conda environment for your project and install all default dependencies 
(configured in `<your new project's folder>/environment.yml`) as well as generate the file 
`<your new project's folder>/environment.lock` containing the exact versions of your dependencies.

You can verify that the environment has been created by finding it among existing environments listed by 

``` bash    
> conda env list
```

**Note:** If you need to delete the environment for any reason, just run

``` bash    
> make delete_environment
```

or 

``` bash    
> conda env remove -n <your conda environment name>
```

### Set up the project in your IDE with the correct conda environment

#### PyCharm

Open PyCharm, go to `File->New Project`, set up `Location` to project's home directory and select
`<path to your conda installation>/envs/<project's conda environment name>/python.exe` as the `Existing interpreter`.

**Alternatively** (in an existing PyCharm project): In Pycharm go to `File->Settings->Project->Python Interpreter`,
click on the `Settings icon` next to the python interpreter path, choose `Add->Conda environment->Existing environment` and set
the `Python interpreter` field to `<path to your conda installation>/envs/<your conda environment name>/python.exe`.

#### Spyder

There are several possible approaches to how to use Spyder with your project's conda environment.
See [here](https://github.com/spyder-ide/spyder/wiki/Working-with-packages-and-environments-in-Spyder#working-with-other-environments-and-python-installations)
for possible Spyder installation options, how to set up the desired conda environment for your project in them, and for more details.

In some of the scenarios you will need to specify your python interpreter. If this will be the case, 
use `<path to your conda installation>/envs/<your conda environment name>/python.exe`.

To verify that you have set up your conda environment correctly, open Spyder and do either of the following:
 - check that the name of the conda environment shown in the status bar is this project's 
conda environment name,
 - run the following in the Spyder's IPython console 
 ```
 import sys
 print(sys.executable)
 ```
 and check that the output is `<path to your conda installation>/envs/<your conda environment name>/python.exe`.

#### Command line

In order to use the project's conda environment in a command line (e.g. a conda prompt), just activate it by
```bash
> conda activate <your conda environment name>
```

### Set up git

#### PyCharm

Note the `<path to git>` for the instance of git that you want to use.

You can use any git instance you like. For example:
- git installed globally in your system (the `<path to git>` is then the path to your `git.exe` file),
- git installed in another conda environment (e.g. 'base'; the `<path to git>` is then `<path to your conda installation>/envs/Library/bin/git.exe`),
- git installed in your project's conda environment (the `<path to git>` is then `<path to your conda installation>/envs/<your conda environment name>/Library/bin/git.exe`).

In PyCharm, go to `VCS->Import into version control->Create Git Repository` and select `<your new project's folder>`.

This initializes your local git repository.

In order to connect a remote GitHub repository, go to `VCS->Import into version control->Share Project on GitHub`,
choose the settings you like and click 'Share'. In the pop-up window select all files, change 'Commit Message' to
*Initialize a new cookiecutter-datascience-ML project* and click 'Add'.

Your first commit has now been pushed to GitHub!

#### Other (via command line)

Open a conda prompt or another shell that can run the git instance you prefer. 
(In a conda prompt the active environment's git will be used.)

Navigate to your project folder and run

``` bash
> git init
> git add .
> git commit -m "Initialize a new cookiecutter-datascience-ML project"
```

Create a new github repository https://github.com/XYZ/abc in your web browser and connect it as a remote to your local repository:

```bash
> git remote add origin https://github.com/XYZ/abc
> git push -u origin master
```

## The next steps

After activating your new conda environment, you are all set up. Here are the next
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