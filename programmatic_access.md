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

You will need access to a [Python](https://www.python.org) installation, either on your computer or on a HPC node. **Note:** Python is installed by default on MacOS and most Linux systems. The examples given here are for Python version 3.7 or higher.

Use `pip` to install the [ckanapi](https://github.com/ckan/ckanapi) extension and the `requests` extension:

```
$ pip install ckanapi
$ pip install requests
```

### Getting started: Bash

You will need [curl](https://curl.haxx.se/) installed. **Note:** Curl is installed by default on MacOS and most Linux systems. For Windows, you can easily install it using the [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/), or [chocolatey](https://chocolatey.org/).

You will also need [jq](https://stedolan.github.io/jq/) installed - it is including in most Linux distributions, and is available via [homebrew](https://brew.sh/) on MacOS.

## Searching the CKAN archive

CKAN makes it possible to query datasets and resources programmatically.

This may be of use when:

- running a specific query repeatedly, or 
- building a front-end or command line tool to search for data in a particular way

It is helpful to be aware of two key concepts which the Data Portal uses:

1. Packages: a package is a collection of zero or more *resources*. It is effectively a flat folder, with metadata.
2. Resources: a resource is a file, with its associated metadata, including its type and size

The following sections contain example code snippets demonstrating how to search for all 'amplicon' data from the Australian Microbiome Framework Initiative. The Bioplatforms Australia Data Portal allows up to 50,000 search results to be returned at a time. The results returned will contain the *package* and *resource* metadata for each 'amplicon' dataset.

The list of all data types, and their associated schemas, are in the [ckanext-bpatheme](https://github.com/BioplatformsAustralia/ckanext-bpatheme/tree/master/ckanext/bpatheme) repository on Github. Each data type has a JSON schema file in that directory.

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
library(ckanr)
key="xx-xx-xx-xx-xx"
ckanr_setup(url="https://data.bioplatforms.com", key=key)
x <- package_search(q='type:amdb-genomics-amplicon', rows=50000, include_private=TRUE)
print(x$count)
```

### Searching: bash

```bash
export CKAN_API_KEY="xx-xx-xx-xx-xx"
curl -H "Authorization: "$CKAN_API_KEY" 'https://data.bioplatforms.com/api/3/action/package_search?q=type:amdb-genomics-amplicon&rows=50000&include_private=true'
```

## Downloading data from the CKAN archive

Once you have identified the packages containing the data you wish to download, it is possible to programmatically
download this data. You may also wish to save the associated metadata for use in your analysis.

The following examples extend the search functionality above to download and save data files.

### Downloading data: Python

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
# iterate through the resulting packages, downloading them one by one
import requests
for package in result['results']:
    for resource in package['resources']:
        url = resource['url']
        resp = requests.get(resource['url'], headers={'Authorization': remote.apikey})
        with open(resource['name'], 'wu') as fd:
            fd.write(resp.content)
```

### Downloading data: R

```R
library(ckanr)
key="xx-xx-xx-xx-xx"
ckanr_setup(url="https://data.bioplatforms.com", key=key)
x <- package_search(q='type:amdb-genomics-amplicon', rows=50000, include_private=TRUE)
headers <- (key)
names(headers) <- c('Authorization')
for (package in x$results) {
    for (resource in package$resources) {
        print(resource$url)
        download.file(resource$url, resource$name, headers=headers)
    }
}
```

### Downloading data: bash

We 'pipe' the response received from the Data Portal into `jq`, a command-line tool which allows us to easily parse JSON data.

```bash
export CKAN_API_KEY="xx-xx-xx-xx-xx"
for URL in $( curl -H "Authorization: $CKAN_API_KEY" 'https://data.bioplatforms.com/api/3/action/package_search?q=type:amdb-genomics-amplicon&rows=50000&include_private=true' | jq -r '.result .results [] .resources [] .url'); do
  # download the file using curl
  echo "downloading: $URL"
  curl -O -L -C - -H "Authorization: $CKAN_API_KEY" "$URL"
done
```

You can also use `jq` to generate an MD5 checksum file, which can be used to verify the downloaded data.

```bash
export CKAN_API_KEY="xx-xx-xx-xx-xx"
curl -H "Authorization: $CKAN_API_KEY" 'https://data.bioplatforms.com/api/3/action/package_search?q=type:amdb-genomics-amplicon&rows=50000&include_private=true' | jq -r '.result .results [] .resources [] | "\(.md5)  \(.name)"' > checksums.md5
# check downloaded data
md5sum -c checksums.md5
```
