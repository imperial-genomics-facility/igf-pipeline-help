---
layout: data_access_template
title: IGF Help pages - data access
---

# Data Access
## Table of Contents

* [Overview](#overview)
* [Browser based file transfer](#browser-based-file-transfer)
* [Command line file transfer](#command-line-file-transfer)
  * [Steps for setting up iRODS client in HPC CX1](#steps-for-setting-up-irods-client-in-hpc-cx1)
  * [Steps for command line transfer in HPC CX1](#steps-for-command-line-transfer-in-hpc-cx1)
* [Access QC report pages](#access-qc-report-pages)
* [List of resources](#list-of-resources)
* [Change logs](#change-logs)

## Overview

A local installation of [iRODS server](http://eliot.med.ic.ac.uk:8080/idrop-web) is used for the data handover to the users. A copy of the data is kept in this server only for a limited time and then automatically removed after the data transfer deadline. Access to this server is restricted by the Imperial College's firewall.
Users are only allowed to access this server, once they are connected to the college's network (either direct or VPN access).

## Browser based file transfer

* Connect to Imperial College network (direct LAN connection or set up [VPN](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/remote-access/method/set-up-vpn/))
* Go to [http://eliot.med.ic.ac.uk:8080/idrop-web](http://eliot.med.ic.ac.uk:8080/idrop-web) and log in using your login credentials

## Command line file transfer

### Steps for setting up iRODS client in HPC CX1 

* Create directory `.irods` under home (e.g. `mkdir -p ~/.irods`)
* Create iRODS environment file `.irods/irods_environment.json`
* Copy following configuration to the above mentioned file (replace USERNAME with your actual username)

<pre><code>
{
      "irods_host": "eliot.med.ic.ac.uk",
      "irods_port":1247,
      "irods_default_resource": "woolfResc",
      "irods_user_name": USERNAME,
      "irods_zone_name": "igfZone", 
      "irods_ssl_ca_certificate_file": "/apps/irods/certs/igf-chain.pem", # Skip if your authentication Type is Standard
      "irods_ssl_ca_certificate_path": "/apps/irods/certs",               # Skip if your authentication Type is Standard    
      "irods_ssl_verify_server": "cert",                                  # Skip if your authentication Type is Standard
      "irods_authentication_scheme": "PAM"                                # Skip if your authentication Type is Standard
}

</code></pre>

* Validate `.irods/irods_environment.json` file format using [JSONLint](https://jsonlint.com/)

### Steps for command line transfer in HPC CX1

* Load irods tool (e.g. `module load irods/4.2.0`)
* Set up your iRODS account using command `iinit` and specify your password
* Download data using commandline tool  `iget` (e.g. `iget -Pr /igfZone/home/USERNAME/PROJECT_NAME/PATH`)

## Access QC report pages

QC report pages for the raw and anlysed data files are accessible from our ftp site and accessible via the following url format `http://eliot.med.ic.ac.uk/report/project/PROJECTNAME`. You have to use the same login credentials for accessing these pages. For more details, please check [Project QC Report](qc_report_page.html) section of the help page.

You can access these pages from your mobile device if you are connected to wifi network [Imperial-WPA](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/wifi-and-networks/access-wifi/).

## List of resources

* [iRODS](https://irods.org/)
* [iRODS server for genomic facility](http://eliot.med.ic.ac.uk:8080/idrop-web)

## Change logs

* None
