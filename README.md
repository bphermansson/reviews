# Reviews

[![Tests](https://github.com/apoclyps/reviews/actions/workflows/test.yml/badge.svg)](https://github.com/apoclyps/reviews/actions/workflows/test.yml)
![pypi](https://img.shields.io/pypi/v/reviews.svg)
![versions](https://img.shields.io/pypi/pyversions/reviews.svg)

![](https://banners.beyondco.de/Reviews.png?theme=light&packageManager=pip+install&packageName=reviews&pattern=plus&style=style_1&description=Monitor+requests+for+Code+Reviews&md=1&showWatermark=0&fontSize=225px&images=link&widths=250)

Simplify requests for code review with an all-in-one TUI dashboard providing an overview of open PRs requiring review!

### Quick Start

If you want to get up and running with Reviews immediately, run:

```bash
export GITHUB_USER="your-github-username"
export GITHUB_TOKEN="your personal GitHub token used for interacting with the API"
export REVIEWS_GITHUB_REPOSITORY_CONFIGURATION="apoclyps/reviews"

pip install --upgrade reviews

reviews config --show

reviews metrics

reviews dashboard --no-reload
```

[![asciicast](https://asciinema.org/a/414444.svg)](https://asciinema.org/a/414444)

### Additional Support

#### View Configuration

If you wish to view the configuration used by reviews at any time, you can use the following command to show all configuration (with secrets hidden or shown):

```bash
reviews config --hide

reviews config --show
```

#### Gitlab

If you wish to use `reviews` with Gitlab, you will need to specify the configuration like so: `product id:project name/repository` and use the `--provider=gitlab` flag when running `reviews`:

```bash
export GITLAB_USER=user
export GITLAB_TOKEN=token
export REVIEWS_GITLAB_REPOSITORY_CONFIGURATION="27629846:apoclyps/reviews"

reviews dashboard --provider=gitlab
```

**Experimental**

#### Github Enterprise

The following features have been implemented but support is considered experimental and it may be removed in a future release.

If you wish to use Reviews with a Github Enterprise instead of Github (https://api.github.com", you can set the `GITHUB_URL` with your custom hostname. You will also need to ensure your `GITHUB_USER` and `GITHUB_TOKEN` exist within your Enterprise Github instance:

```bash
export GITHUB_URL=https://{hostname}/api/v3
export GITHUB_USER=user
export GITHUB_TOKEN=token
```

### Getting started with local development

To build and run the CLI on your host, you will need Python 3.9, pip, and virtualenv to build and run `review`:

```bash
$ python3 -m venv env
$ source env/bin/activate
(env)$ pip install -r requirements_dev.txt
(env)$ pip install -r requirements.txt
(env)$ python -m reviews dashboard
```

If you wish to keep a copy of Reviews on your host system, you can install and run it using:

```bash
$ export REVIEWS_GITHUB_REPOSITORY_CONFIGURATION="apoclyps/reviews"
$ python -m venv env
$ source env/bin/activate
$ python -m pip install -e .
$ reviews -h
```

You can run the Reviews within Docker:

```bash
docker-compose build cli && docker-compose run --rm cli python -m reviews dashboard
```

To build an image and run that image with all of the necessary dependencies using the following commands:

```bash
$ docker-compose build cli
$ docker-compose run --rm cli python -m reviews dashboard
```

For instructions on setting up a development environment outside of Docker, check out the [wiki](https://github.com/apoclyps/reviews/wiki/Development-Enviromnent).

### Configuration

Reviews supports both .ini and .env files. Reviews always searches for Options in this order:

* Environment variables;
* Repository: ini or .env file;
* Configuration Path
* Review Defaults

#### Ini file
Create a `settings.ini` next to your configuration module in the form:

```bash
[settings]
REVIEWS_GITHUB_REPOSITORY_CONFIGURATION=apoclyps/micropython-by-example
Note: Since ConfigParser supports string interpolation, to represent the character % you need to escape it as %%.
```

#### Env file
Create a `.env` text file on your repository's root directory in the form:

```bash
REVIEWS_GITHUB_REPOSITORY_CONFIGURATION=apoclyps/micropython-by-example
```

#### Providing a configuration path

If you wish to set the configuration path to use an `ini` or `.env` file when running the application, you can use the configuration of a specific file by supplying the path to the configuration like so:

```bash
export REVIEWS_PATH_TO_CONFIG=/home/apoclyps/workspace/apoclyps
```

If at any time, you want to confirm your configuration reflects the file you have provided, you can use `reviews config` to view what current configuration of Reviews.

### Testing

A test suite has been included to ensure Reviews functions correctly.

To run the entire test suite with verbose output, run the following:

```bash
$ pytest -vvv
```

Alternatively, to run a single set of tests.

```bash
$ pytest -vvv tests/test_config.py
```

All tests can be run within docker by using the following command:

```bash
$ docker-compose build pytest && docker-compose run --rm pytest
```

### Linting

To run individual linting steps:

```
docker-compose build test
docker-compose run --rm --no-deps test isort .
docker-compose run --rm --no-deps test black --line-length 119 --check .
docker-compose run --rm --no-deps test mypy .
docker-compose run --rm --no-deps test flake8 .
docker-compose run --rm --no-deps test pylint --rcfile=.pylintrc reviews
docker-compose run --rm --no-deps test bandit reviews
docker-compose run --rm --no-deps test vulture --min-confidence 90 reviews
docker-compose run --rm --no-deps test codespell reviews
docker-compose run --rm --no-deps test find . -name '*.py' -exec pyupgrade {} +
```

You can also set up ``pre-commit`` to run the linting steps automatically during the commit phase,
the pre-commit pipeline can be set up by running the following command on the project root:
```bash
pre-commit install
```

### Contributions

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.
