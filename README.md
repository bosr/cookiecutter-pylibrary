# Cookiecutter template for Python library with Jenkins integration

Modified (simplified) version from [ionelmc great template](https://github.com/ionelmc/cookiecutter-pylibrary) for usage with self-hosted Jenkins:

    - no cloud-based services (coveralls, etc)
    - pylint, coverage, tox with Py2.7 and Py3.6
    - Jenkins

I did this because I have to work on some projects that are not allowed to use cloud-based services.

## Usage
The following assumes you have `cookiecutter` and `tox` available in your `PATH` (I don't install them per-project, but YMMV).

As usual, instantiate the template (default name `python-nameless`):

    cookiecutter https://github.com/bosr/cookiecutter-pylibrary

Navigate to the instantiated template (`python-nameless`):

    cd python-nameless

The template uses `tox` and `pytest`, so basically, running the tests is simple:

    tox

Notice the presence of a `Jenkinsfile`, which basically calls `tox` and makes the following available to Jenkins

    - coverage reports,
    - HTML coverage reports
    - pylint warnings are automatically generated for each build (see `Jenkinsfile`).
