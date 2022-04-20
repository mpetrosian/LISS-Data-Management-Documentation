.. _implementation:

**************
Implementation
**************

Cleaning Steps
==============

The script performs the following steps for each study that is specified in `specification/data_sets_specs.json`:

1. Collect all .dta files in the specific folder (e.g. `data/002-health/`).
2. Rename all variables according to the respective renaming file. (e.g. `specifications/002-health_renaming.csv`).
3. Put all waves together for a panel data set.
4. Replace values of variables (for some data sets) based on replacing file.
5. Set types of variables (either as specified in the renaming file or infered by the values of the variable).
4. Perform some further data cleaning.
5. Save as .pickle, .dta, and/or .csv

See `pytask documentation <https://pytask-dev.readthedocs.io/en/latest/>`_ for more information on the project builder that is used.

Questionnaire-specific Cleaning
===============================

The data cleaning is specified in `data_management/clean_one_dataset.py`

The script controls the steps described above. While some steps like the renaming are done for each study, there is also data cleaning specifically for each study. These specific data cleaning functions are named `clean_{file_name}`.

In the end, usually each cleaned variable should be of one of the following types:

- Dummy variable (coded as float)
- Numerical (only numerical values and missing)
- Categorical (usually only string labels and missing)
- Ordered Categorical (only string labels with logical order and missing)
- Object (only if is some kind of free text question)

Background data is available for every month. The prepared background files contain a monthly file for each year, as well as a yearly file in which the data is aggregated on the year level.

Renaming Files
==============

- The renaming files are ";"-separated .csv files and specify the new name for each variable.
- The first column `new_name` specifies the new variable name, the next specifies the variable lable (in the STATA file). All remaining columns show the raw_variable names - one column for each wave. The respective file name is equivalent to the column name.
- Variables in different waves in the same row get the same name.
- Variables for which no new name is specified, are dropped.

Replacing Files
===============

- The renaming files are ``yaml``-files and specify how the values of variables should be renamed.
- They do not exist for all data sets.
