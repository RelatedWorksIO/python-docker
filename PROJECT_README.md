This repo is a portable, re-runnable, snapshot of the analytics investigation Related Works did for Octopart. It's mostly python, pandas, matplotlib, and Jupyter.

# Setup
The only software you need to install is [Docker](https://www.docker.com/).

## Analytics Submodule

RW has a library of python code and docker scaffolding that's shared across clients and lives as a git submodule. You'll need to check that out.

If you haven't cloned this repo yet, you can do a recursive clone:
```
git clone --recursive <git repo url>
```

Otherwise you'll have to do a git submodule update at the root of this repo:
```
git submodule update --init
```

## Build the Container
The `./docker-scripts/build` script will build a container named according to the project:
```
± ./docker-scripts/build
+ docker build --tag X_notebooks .
Sending build context to Docker daemon  52.22MB
Step 1/13 : FROM 'jupyter/datascience-notebook'
 ---> 93936cc74dd8
Step 2/13 : USER root
 ---> Using cache
 ---> ace15739f1ea
...
```

## Secrets
Passwords and other secrets are transmitted to the container via a git-ignored  `secrets/env` file. Ask Gio to securely transmit this file to you.

Make sure to to restrict it's permissions on the filesystem:
```
± chmod 600 secrets/env
```

## Run the Jupyter Server
Now you should be able to run the Jupyter server:
```
± ./docker-scripts/notebook
...
[I 18:20:53.553 NotebookApp] The Jupyter Notebook is running at:
[I 18:20:53.553 NotebookApp] http://0.0.0.0:8888/
```
You should be able to open your browser and hit [localhost:8888](http://localhost:8888/tree?).


# Scripts

There are a number of convenient scripts at the root of the repository:

* `./docker-scripts/build` builds the container.
* `./docker-scripts/notebook` runs the notebook server.
* `./docker-scripts/shell` will give you a bash shell in the container.
* `./docker-scripts/ipython` will give you an IPython shell in the container with the appropriate python path.
* `./docker-scripts/test` will run all the tests.
* `./docker-scripts/format` will sort imports and format all of the code using [black](https://github.com/ambv/black).
* `./docker-scripts/check-types` will validate type hints using [mypy](http://mypy-lang.org/).


# Directory Structure

* `docker-scripts` is where all the scripts that run docker containers live. They do things like run a notebook server, the tests, etc.
* `analytics-core` is RW's shared library, where you'll find common functions to pull data from databases, style vizualizations, etc.
* `notebooks` is where all the Jupyter Notebooks live. This tends to be the entry-point for analysis.
* `analysis` is where the core analysis logic lives. Jupyter notebook can quickly become a mess so I tend to put all logic in separate python modules.
* `lib` is where re-usable library code, like fetching and formatting common datasets, specific to this project live
* `sql` is where all the sql files live.
* `tests` is where all the tests live, in a directory-structure that mirrors the outer repo.
* `static_data` is for storing small static datasets
* `data` is a git-ignored directory for storing large input/output files, typically CSVs.
* `.disk_cache` is a git-ignored directory for caching the results of complex computations and SQL queries.

