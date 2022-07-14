# Using OAM w/ Docker

Once the OAM docker image is installed, we will rely mostly on docker compose as an engine for running OAM in different ways. Each of these examples is available in this repo.

To use docker compose, we create a .yml file specifying how we will be using docker, then run the jobs with the following command:

```bash
docker compose -f [path to yml file] up
```

### Running Jupyter Notebook/Jupyter Lab

```yaml
jupyter:
  image: oam-release:latest
  ports:
    - 8888:8888
  volumes:
		- ".:/home/notebook"
  command: >
    bash -c "pip install jupyter
    && cd /home/notebook
    && jupyter notebook --port=8888 --allow-root --no-browser --ip='*'"
```

The above docker-compose configuration will install jupyter and run a jupyter notebook from the server with access to all of the files in your current directory.

If you prefer `jupyter-lab` then you can swap out the command as follows:

```bash
jupyter:
  image: oam-release:latest
  ports:
    - 8888:8888
  volumes:
		- ".:/home/notebook"
  command: >
    bash -c "pip install jupyter
    && cd /home/notebook
    && jupyter lab --port=8888 --allow-root --no-browser --ip='*'"
```

### Running Bash

```bash
bash:
  image: oam-release:latest
  volumes:
    - ".:/home/workspace"
  working_dir: /home/workspace
```

Running a bash terminal might be important for some quick experimenting or if you need to run some code in python. However, to get an interactive shell the command for running the docker compose changes slightly to become:

```bash
docker-compose -f bash.yml run --rm bash
```

Note the addition of `--rm bash`

### Adding on with your own code

This is pretty simple. Just use one of the above methods (bash or jupyter notebook), in a folder that contains the new code you want to add on. It will be made available in bash and jupyterlab/jupyter notebook when you run it. These modules can import and use olorenautoml without any issues.

### View Docs

```yaml
docs:
  image: oam-release:docs
  ports:
    - 1235:1235
  volumes:
    - ".:/home/workspace"
  working_dir: /usr/src/olorenautoml/docs/_build/html
  command: python3 -m http.server 1235
```
