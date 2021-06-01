.. This the LISS Panel Data Management project's documentation master file.

.. You can adapt this file completely to your liking,
.. but it should at least contain the `toctree` directive.


Welcome to the LISS Data Management project's documentation!
============================================================

Documentation on the `liss-data` repository. This repository cleans the datasets of the LISS (Longitudinal Internet Studies for the Social sciences) panel. It is written in Python.


About LISS Panel
----------------

The `LISS panel <https://www.lissdata.nl/>`_ is an internet-based household panel administered by CentERdata (Tilburg University, The Netherlands). The sample is drawn from the Dutch population via random sampling. Every month, participants answer questionnaires that are exclusively reserved for research purposes. Respondents are financially compensated for all questions they answer.

Motivation
---------- 

Data cleaning is an important component of empirical research projects. The goal of this repository is to share the effort associated with the most basic parts of data cleaning.

The basic idea is that all fundamental data cleaning that is helpful in (almost) all projects using this data set should be done within the liss-data repo. Examples are the renaming of variables, renaming of values (e.g. "I don't know -> np.nan"), or the calculation of simple variables that are often used (e.g. `has_risky_financial_assets = risky_financial_assets > 0`).

Conversely, specific data management that is mostly relevant for your project, in particular the merging of data sets and selecting of specific observations, should be done outside of this repo in your own project folder.


.. toctree::
    :maxdepth: 2

    getting_started
    implementation
    advanced_usage
    people
