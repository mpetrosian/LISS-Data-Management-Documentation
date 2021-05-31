# Data Cleaning for LISS panel

## LISS panel

The LISS (Longitudinal Internet Studies for the Social sciences) panel is an internet-based household panel administered by CentERdata (Tilburg University, The Netherlands).

To use this repository (and get access to LISS panel files) you must register here first: [https://www.dataarchive.lissdata.nl/](https://www.dataarchive.lissdata.nl/)

## Motivation

- Data cleaning is an important component of empirical research projects.
- But it can be very tedious.
- Cleaned data sets can be used for several different research projects → public good
- The goal of this repository is to share the effort associated with the most basic parts of data cleaning.

## Usage

There are three prototypical ways how to use the repository:

1. **Raw Files:** You can use this repository just to have an easy access to the raw LISS files all in one place. It would be sad, however, if you stopped here.
2. **Prepared Files:** For many use cases it is more efficient if you make use of the already implemented data cleaning and use the prepared files. For this, you need to run the pipeline using waf. Just select your preferred format (.dta, .csv, .pickle) in `data_management/wscript.py`. You find the prepared files under `out/data`.
3. **Implement your own changes:** Once you start to use the prepared files, it is not unlikely that you would like to change something (possible due to errors, or incompletely cleaned variables). Feel free to change the renaming files, adjust the specific cleanings for a given study, or add new waves to your study. In this case, you need to know about the precise implementation details and you should continue reading. You will you need to know some Python.

The basic idea is that all fundamental data cleaning that is helpful in (almost) all projects using this data set should be done within the liss-data repo. Examples are the renaming of variables, renaming of values (e.g. "I don't know -> np.nan"), or the calculation of simple variables that are often used (e.g. `has_risky_financial_assets = risky_financial_assets > 0`). Special data management that is mostly relevent for your project, in particular the merging of data sets and selecting of specific observations, should be done outside of this repo in your own project folder.

## Setup

Make sure you have your conda environment up to date. Basic requirements are specified in the `environment.yml` file.

- run `python waf.py configure`
- run `python waf.py`

This resource can be helpful to get an understanding of waf: [https://econ-project-templates.readthedocs.io/en/stable/py-example.html#introduction-to-waf](https://econ-project-templates.readthedocs.io/en/stable/py-example.html#introduction-to-waf)

To also create the additional corona questionnaire files used by the CoViD-19-Impact-Lab run:

- `python waf.py --corona_prep=1 install`

When using the sandbox, you might need to add or remove (multiple) ../ at the beginning of the notebook in the path.

## Structure

- `data/` contains all data sets of the LISS panel that are publicly available (if you miss something feel free to add it). For each questionnaire, we collected (1) the English codebook as .pdf and (2) the Stata data file. Each folder contains either these files directly (if it is a Single Wave Study) or subfolders for each wave "wave-1", "wave-2", etc.
- `specifications/` contains several renaming files that specify the new variable names for each study. In addition, the file `specification/data_sets_specs.json` lists all studies that should be cleaned and specifies more information about these studies (like the file name).
- `data_management/` contains the files for the data cleaning.

The repository only contains raw files and scripts. All output files need to be produced by running waf and can then be found under `out/data`.

## Implementation Details

The script performs the following steps for each study that is specified in `specification/data_sets_specs.json`:

1. Collect all .dta files in the specific folder (e.g.`data/002-health/`).
2. Rename all variables according to the respective renaming file. (e.g. `specifications/002-health_renaming.csv`).
3. Put all waves together for a panel data set.
4. Perform some data cleaning.
5. Save as .pickle, .dta, and/or .csv

See [https://econ-project-templates.readthedocs.io/en/stable/](https://econ-project-templates.readthedocs.io/en/stable/) for more information on the template that is used.

### Renaming Files

- The renaming files are ";"-separated .csv files and specify the new name for each variable.
- The first column `new_name` specifies the new variable name, the next specifies the variable lable (in the STATA file). All remaining columns show the raw_variable names - one column for each wave. The respective file name is equivalent to the column name.
- Variables in different waves in the same row get the same name.
- Variables for which no new name is specified, are dropped.

### Data Cleaning

The data cleaning is specified in `data_management/clean_one_dataset.py`

The script controls the steps described above. While some steps like the renaming are done for each study, there isalso data cleaning specifically for each study. These specific data cleaning functions are named `clean_{file_name}`.

In the end, usually each cleaned variable should be of one of the following types:

- Dummy variable (coded as float)
- Numerical (only numerical values and missing)
- Categorical (usually only string labels and missing)
- Ordered Categorical (only string labels with logical order and missing)
- Object (only if is some kind of free text question)

Background data is available for every month. The prepared background files contain a monthly file for each year, as well as a yearly file in which the data is aggregated on the year level.

## Adding new files

To add new studies or new waves of existing studies, please follow the following steps:

- Download both the respective data file as .dta and the English codebook as .pdf
- Put those files in `data/` and make sure the folder structure is consistent to the previous studies
- You can add `_do_not_use.dta` at the end of a .dta file to make sure it is not considered in the cleaning process
- If it's a completely new study (not just a new wave), adjust `data/specification/data_sets_specs.json` accordingly
- Run `create_renaming_files` from the sandbox to automatically add the new columns in the respective renaming files
- Manually adjust the renaming file
- Run waf

## Contributing

- The code for the data cleaning was mostly written by Moritz Mendel and Christian Zimpelmann. Valuable contributions have been made by Hans-Martin von Gaudecker, Renske Stans, Yasemin Özdemir, Axel Wogrolly, Arthur Murschel, Eva Justenhoven, Floren Pfann, and Felipe Azuero.
- We would be more than happy if more people find this repository useful, start using it, and contribute improvements many of which are still possible.
- Feel free to implement minor changes in the data management yourself and talk to us about major changes.
