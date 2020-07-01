# Programmatic access to the Bioplatforms Data Portal

The Bioplatforms Data Portal is based upon [CKAN](https://ckan.org/), an open source platform for sharing metadata and data. CKAN provides Application Programming Interfaces (APIs) which allow computer software to interact with the portal. It is possible for software to programmatically do everything you can do manually by using the portal in your web browser - including searching for data, downloading data, and even uploading data.

This guide covers access to the portal using the R and Python programming languages, and via the command shell on your desktop computer (`bash` or `zsh`.)

## Getting started

When your computer program connects to the portal, it must identify itself. It will do this by sending an 'API key'. You can find this key on the portal.

1. Go to the portal https://data.bioplatforms.com/
2. At the top right of the page, you will see either a prompt to log in, or your name 
3. Log in, if required, and then click on your name
4. Your API key will be visible in the sidebar

If you are downloading or uploading large datasets from the portal, it is highly recommended that you do this from an appropriate environment, with a fast and reliable connection to the internet. Many institutions provide access to HPC nodes.

### Getting started: R

You will need access to an [R](https://www.r-project.org) installation, either on your computer or on a HPC node.

1. Launch R, and 
2. Install the [ckanr](https://github.com/ropensci/ckanr) bindings for CKAN:

```
> install.packages("ckanr")
```

### Getting started: Python

You will need access to a [Python](https://www.python.org) installation, either on your computer or on a HPC node. **Note:** Python is installed by default on MacOS and most Linux systems.

Use `pip` to install the [ckanapi](https://github.com/ckan/ckanapi) extension:

```
$ pip install ckanapi
```

### Getting started: Bash

You will need [curl](https://curl.haxx.se/) installed. **Note:** Curl is installed by default on MacOS and most Linux systems. For Windows, you can easily install it using the [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/), or [chocolatey](https://chocolatey.org/).

## Searching the CKAN archive

CKAN makes it possible to query datasets and resources programmatically.

This may be of use when:

- running a specific query repeatedly, or 
- building a front-end or command line tool to search for data in a particular way

It is helpful to be aware of two key concepts which the Data Portal uses:

1. Packages: a package is a collection of zero or more *resources*. It is effectively a flat folder, with metadata.
2. Resources: a resource is a file, with its associated metadata, including its type and size

The following sections contain example code snippets demonstrating how to search for all 'amplicon' data from the Australian Microbiome Framework Initiative. The Bioplatforms Australia Data Portal allows up to 50,000 search results to be returned at a time. The results returned will contain the *package* and *resource* metadata for each 'amplicon' dataset.

### Searching: Python

```python
import ckanapi
remote = ckanapi.RemoteCKAN('https://data.bioplatforms.com', apikey='xx-xx-xx-xx-xx')
# we increase the number of rows to be returned, and we
# ask for all packages, including private packages
result = remote.action.package_search(
    q='type:amdb-genomics-amplicon',
    rows=50000,
    include_private=True)
print("{} matches found.".format(result['count']))
```

### Searching: R

```R
ckanr_setup(url="https://data.bioplatforms.com", key="xx-xx-xx-xx-xx")
x <- package_search(q='type:amdb-genomics-amplicon', rows=50000, include_private=TRUE)
x$count
```

### Searching: bash

```bash
curl -H 'Authorization: xx-xx-xx-xx-xx' 'https://data.bioplatforms.com/api/3/action/package_search?q=type:amdb-genomics-amplicon&rows=50000&include_private=true'
```