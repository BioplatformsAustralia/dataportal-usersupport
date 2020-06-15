## **Find, filter and download Framework Initiative data**

1. Go to the Data portal page
2. Log-in using your username and password (created during [registration](registration_login.md))
     - Login is required for full functionality including access to datasets
3. Select the banner or text link for the Framework Initiative of interest: e.g. [Oz Mammals Genomics](https://ozmammalsgenomics.com/)

![](/omg_banner.png)

4. Use the Search datasets… field  textbox to search for the data you want (e.g. PacBio Dunnart)
5. To narrow search results further, use the Tags in the left column, which will filter the data (see image at right)
6. When you have the files you want on the search page, click bulk download

     - This will generate a zip folder with the files you need to download the data
     - Download and decompress this folder
     - Inside there are the following files and folders:
          - package_metadata/
          - resource_metadata/
          - download.ps1
          - download.sh
          - md5sum.txt
          - README.txt
          - urls.txt

7. README.txt provides instructions for data download: PLEASE READ THIS!
8. package_metadata contains a spreadsheet file with the metadata relevant to the downloaded filtered data set
9. resource_metada contains a spreadsheet file with the metadata relevant to the files which comprise the filtered data set
10. download.ps1 and download.sh are shell scripts 
     - download.ps1: Windows PowerShell script, which when executed will download the files, and then checksum them. This is supported on a Microsoft system, and uses only PowerShell.
     - download.sh: UNIX shell script, which when executed will download the files, and then checksum them. This is supported on any Linux or MacOS/BSD system, so long as `curl` is installed.
 
 ```
 #!/bin/sh

#
# This UNIX shell script was automatically generated.
#

if ! which curl >/dev/null 2>&1; then
  echo "`curl` is not installed. Please install it."
  echo
  echo "On MacOS, it can be installed via HomeBrew (https://brew.sh/)"
  echo "using the command `brew install curl`"
  exit 1
fi

echo "Downloading data"
if [ x"$CKAN_API_KEY" = "x" ]; then
    while read URL; do
        echo "Downloading: $URL"
        curl -O -L -C - "$URL"
    done < urls.txt
else
    while read URL; do
        echo "Downloading: $URL"
        curl -O -L -C - -H "Authorization: $CKAN_API_KEY" "$URL"
    done < urls.txt
fi

echo "Data download complete. Verifying checksums:"
md5sum -c md5sum.txt 2>&1 | tee md5sum.log
```

11. md5sum.txt and urls.txt contain the checksum and url data locations respectively
12. When you run download.sh, it will provide instructions to set up your API key
13. Set up API key
14. Run downloads.sh again
15. The data should now download and checksum
