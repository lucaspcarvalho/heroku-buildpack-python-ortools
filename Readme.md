# What is this fork?
This is a fork of the official Python buildpack that supports the Python bindings for [Google's `or-tools`](https://github.com/google/or-tools). 

`ortools` does not install smoothly via `pip` directly. This buildpack adds a compile script to install `ortools` via `setuptools` based on the installation instructions here: https://developers.google.com/optimization/installing#python

To trigger this extra script, simply include `ortools` in your project's `requirements.txt`.

The version of `ortools` is pinned in `vendor/ortools/setup.py` in an effort to make builds deterministic and predictable.

Follow the instructions below for setting a custom buildpack for your Heroku app below and then confirm that everything is working by running the following commands:

```shell
heroku run python
>>> from ortools.linear_solver import pywraplp
```

If you don't get an `ImportError`, you are good to go :)

This buildpack has been tested with Python 2.7 -- `ortools` has a separate Python 3 package but that may require modification to the buildpack. Here be dragons...

# Heroku Buildpack: Python with `ortools`
![python-banner](https://cloud.githubusercontent.com/assets/51578/8914205/ecf2047c-346b-11e5-98c5-42547f9f4410.jpg)

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for Python apps, powered by [pip](http://www.pip-installer.org/).

This buildpack supports running Django and Flask apps.


Usage
-----

Example usage:

    $ ls
    Procfile  requirements.txt  web.py

    $ heroku create --buildpack git://github.com/swanson/heroku-buildpack-python-ortools.git

    $ git push heroku master
    ...
    -----> Python app detected
    -----> Installing runtime (python-2.7.11)
    -----> Installing or-tools
    -----> Installing dependencies using pip
           Downloading/unpacking requests (from -r requirements.txt (line 1))
           Installing collected packages: requests
           Successfully installed requests
           Cleaning up...
    -----> Discovering process types
           Procfile declares types -> (none)

You can also add it to upcoming builds of an existing application:

    $ heroku buildpacks:set git://github.com/swanson/heroku-buildpack-python-ortools.git

The buildpack will detect your app as Python if it has the file `requirements.txt` in the root.

It will use Pip to install your dependencies, vendoring a copy of the Python runtime into your slug.

Specify a Runtime
-----------------

You can also provide arbitrary releases Python with a `runtime.txt` file.

    $ cat runtime.txt
    python-3.5.1

Runtime options include:

- python-2.7.11
- python-3.5.1
- pypy-4.0.1 (unsupported, experimental)
- pypy3-2.4.0 (unsupported, experimental)

Other [unsupported runtimes](https://github.com/heroku/heroku-buildpack-python/tree/master/builds/runtimes) are available as well.
