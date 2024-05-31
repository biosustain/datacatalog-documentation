# Welcome to Biosustain Data Catalog User Guide

This user guide covers using Data Catalog's web interface to organize, publish and find data within Biosustain. All the data and metadata stored in this Data Catalog are only made available to Biosustain employees and guests.

## The Purpose of Data Catalog

The Data Catalog aims to store and manage active data, making it as open as possible and as close as necessary **within DTU Biosustain**. 

Specific goals include:

- Provide a centralized data portal to enhance FAIR data principals throughout the data life cycle.
- Enable data/metadata capture, data enrichment, efficient and standardized automated workflows.
- Provide visibility and overview of accessible data.
- Ensure governance on all systems and data.
- Establish a strong research data management support function.

## Platform Technology - CKAN

[**CKAN**](https://ckan.org/) is one of the most popular open source data management system used by governments and organizations such as [Open Data DK](https://www.opendata.dk/) and [US government Open Data](https://data.gov/). CKAN also has a powerful API (Application Programming Interface), which makes it possible and relatively easy to develop extensions and integrate with our informatics platform at Biosustain.

## Dataset and Resources

In the data catalog powered by CKAN, data is published in units called **"datasets"**. A dataset is a parcel of data - for example, it could be the DNA sequencing reads of an experiment bacterial strain, the growth profile of cells for a fermentation experiment, or statistical analyses of gene expressions. When you search for data, the search results you see will be individual datasets.

A dataset contains two things:

- Information or **“metadata”** about the datasets. For example, common metadata such as the *title* and *creator*, *date-of-creation*, *format*, etc. Also some dataset type specifics such as ***raw data***, ***results***, or  it is available in, what license it is released under, etc.
- A number of **“resources”**, which hold the data (file) itself. A resource can be a CSV or Excel spreadsheet, XML file, PDF document, image file, sequencing fastq, metabolic model file, an url (linking to files), etc. A dataset can contain any number of resources. For example, different resources might contain the data for different experiments, or they might contain the same data in different formats.

## Members, Projects and Authorization

Biosustain Data Catalog, by design, is **only accessible** by personnel officially employed or associated with DTU Biosustain. Each dataset is owned by a project. Project administrators can add individual members to it, with different roles depending on the level of authorization needed. A member in a project can create a dataset owned by that project. This dataset can be set ***private*** which is visible only to other members in the same project, or ***public*** which is visible to all members in Biosustain Data Catalog. 
