# Bulk Downloads from the Bioplatforms Data Portal

Many of the datasets available from the Bioplatforms Data Portal are large in size - up to a terrabyte each.
The portal provides additional functionality for accessing these datasets, beyond simply using the download
functionality in your web browser. This allows for downloads to be resumed if a connection error occurs. It
also allows the download to be run on appropriate infrastructure: if you are making use of cloud or HPC servers,
you can run the download directly onto that system.

To begin, query CKAN for the data you wish to download. Access the framework data initiative you are working with,
and enter a search term. Once the results are displayed, click the button labelled "Bulk download (metadata and data)"
and a Zip file will be downloaded.

This Zip contains scripts (both for `bash` and `powershell`) which allow you to download all of the data, and
confirm the integrity of the downloaded data using checksums.

To run the scripts, refer to the `README.txt` file contained in the Zip archive. If you wish to download the data
from your cloud or HPC environment, copy the Zip archive to that system, extract it, and run the download from there
directly. This means you will have access to the reliable, high-speed connection to the internet provided by that
environment, as well as any attached storage resources.
