---
layout: data_access_template
title: IGF help pages - Data Access
---

# Data Access
## Table of Contents

* [Data access guide](#data-access-guide)
* [Globus based transfer](#globus-based-transfer)
* [Imperial College Research Data Store based transfer](#imperial-college-research-data-store-based-transfer)
  * [How to remove IGF user from the RDS allocation](#how-to-remove-igf-user-from-the-rds-allocation)
* [Illumina Basespace Sequence Hub based file transfer](#illumina-basespace-sequence-hub-based-file-transfer)
* [Data access via iRODS server (Discontinued)](#data-access-via-irods-server-discontinued)
  * [Command line file transfer](#command-line-file-transfer)
    * [Steps for setting up iRODS client in HPC CX1](#steps-for-setting-up-irods-client-in-hpc-cx1)
      * [Authentication Type: Standard](#authentication-type-standard)
      * [Authentication Type: PAM](#authentication-type-pam)
    * [Steps for command line transfer in HPC CX1](#steps-for-command-line-transfer-in-hpc-cx1)
* [Access QC report pages](#access-qc-report-pages)
* [List of resources](#list-of-resources)
* [Change logs](#change-logs)

## Data access guide
**Currently, Globus is our preferred mode of data transfer.**

<span style="color:crimson"><b>Update:</b> We have stopped using iRODS server for data distribution from <b>October 2022</b>.</span>

Check our new data access guide: [Accessing data from IGF](slide_htmls/accessing_data_files.html)

<details>
<summary>Click to expand slides here</summary>

<iframe src="https://imperial-genomics-facility.github.io/igf-pipeline-help/slide_htmls/accessing_data_files.html" width="600px" height="400px" title="Accessing data from IGF"></iframe>
</details>

## Globus based transfer
Imperial College's Research Data Store is now linked to [Globus](https://www.globus.org/) which allowes the following options

* __Transfer__ large volumes of data between the RDS, your personal computer and Globus-accessible storage at other institutions
* __Share__ RDS project allocation data with selected third parties, without requiring them to have a College account (Globus identity required)

Check our new slides for Globus transfer: [globus data transfer](slide_htmls/globus_data_transfer.html)

<details>
<summary>Click to expand slides here</summary>

<iframe src="https://imperial-genomics-facility.github.io/igf-pipeline-help/slide_htmls/globus_data_transfer.html" width="600px" height="400px" title="Globus based transfer"></iframe>
</details>
<p/>
### Requirements
##### For users from Imperial College London
We will require your Colleges username (e.g., `username@ic.ac.uk`) for this mode of data sharing. Please note that data sharing will fail if you provide us alternate user names (e.g. <span style="color:crimson">following will not work</span> `your.fullname@imperial.ac.uk` or `user.name@ic.ac.uk` ). For more details, please have a look at Imperial College's guideline for Globus data transfer: [Transferring data to other sites with Globus](https://icl-rcs-user-guide.readthedocs.io/en/latest/rds/transferringdata/globus/)


##### For users without Imperial College account
Please send us the email id linked to your [Globus](https://www.globus.org/) account.

### Globus transfer process
* We will create a new Globus collection after receiving the user’s request and copy existing data to it.
* The new collection will be shared with the user’s Globus account (i.e., email id or username). We can add more than one user to the same Globus collection.
* User needs to follow [Globus](https://www.globus.org/) or [Imperial College’s documentation](https://icl-rcs-user-guide.readthedocs.io/en/latest/rds/transferringdata/globus/) and transfer data (to any preferred storage location).
* Files from any new sequencing run or analysis will get added to the existing Globus collection directory.
* User needs to transfer any new batch of data separately after receiving email notification from us.
* Each file on the Globus collection directory will be removed after 30 days of file creation.
* We may remove old Globus collections without any prior notice, once all the files are removed from the collection directory.


<div align="right"><a href="#table-of-contents">Go to Top</a></div>

## Imperial College Research Data Store based transfer
<p><b>Please note: We can only use RDS transfer at the very end of the project when we have data from all the sequencing runs and analysis pipelines</b></p>

Imperial College now offers a new central service for storing large volume of research data. Please follow these steps to setup a new storage volume for your sequencing project:

<p style="text-indent: 15px"><b>Step 1:</b> Check the documentation about <a href="https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/service-offering/rds/">Research Data Store</a> (and <a href="https://icl-rcs-user-guide.readthedocs.io/en/latest/rds/">wiki page</a>) and setup a new allocation for your peoject. Also, we have few slides regarding <a href="slide_htmls/setting_up_rds_project.html">setting-up RDS project allocation</a></p>

<details>
<summary>Click to expand slides here</summary>

<iframe src="https://imperial-genomics-facility.github.io/igf-pipeline-help/slide_htmls/setting_up_rds_project.html" title="setting-up RDS project allocation" width="600px" height="400px"></iframe>
</details>
<p/>
<p style="text-indent: 15px"><b>Step 2:</b> Add <b>Imperial BRC Genomics Facility</b> (username: <b>igf</b>) as a new member of the research data storage, once its available</p>
<p style="text-indent: 15px"><b>Step 3:</b> Update IGF regarding your new RDS storage path in HPC</p>
<p style="text-indent: 15px"><b>Step 4:</b> Data will be copied to the top level of the storage using the layout <code>RDS_PATH/live/PROJECT_NAME</code></p>
<p style="text-indent: 15px"><b>Step 5:</b> Remove IGF user from the RDS allocation when all the sequencing runs are finished and data transfer is over <span style="color:#ff0000">(<b>IMPORTANT</b>)</span></p>


### How to remove IGF user from the RDS allocation
Follow these steps for removing IGF user from the RDS allocation:

* Login to [RCS selfservice portal](https://selfservice.rcs.imperial.ac.uk/groups/manage/rds/) portal using your Imperial College credentials
* Click on the **Research Data Storage projects** on the left panel
* Click on the correct **"rds-xyz"** id to access the 'Membership" info for the selected RDS project
* Check if you have admin priviledges for this project or not (i.e., if **Admin?** column has **yes** value or not)
* Go to the row which has entry for user 'igf' and select the checkbox for **Remove?** column and click the **Update** button at the bottom of this page



<div align="right"><a href="#table-of-contents">Go to Top</a></div>


## Illumina Basespace Sequence Hub based file transfer (Discontinued)
<p><b>Please note: We can only transfer fastq files via Basespace.</b></p>

Fastq files from the sequencing runs can be uploaded to [Illumina BaseSpace Sequence Hub](https://www.illumina.com/products/by-type/informatics-products/basespace-sequence-hub.html) based on your request. Following information are required for this specific mode of data transfer:

* Your basespace account email (existing account or a new free basic subscription account)
* Confirmation regarding the sample consent type

<b>BaseSpace configuration:</b>

* We use [Basespace London region](https://euw2.sh.basespace.illumina.com) for data upload and share
  * API server: `https://api.euw2.sh.basespace.illumina.com`

* Data can be download via [Basespace cli](https://developer.basespace.illumina.com/docs/content/documentation/cli/cli-overview) or via [browser](https://euw2.sh.basespace.illumina.com)

<div align="right"><a href="#table-of-contents">Go to Top</a></div>


## Data access via iRODS server (Discontinued)

A local installation of iRODS server is used for the data handover to the users. A copy of the data is kept in this server only for a limited time and then automatically removed after the data transfer deadline. Access to this server is restricted by the Imperial College's firewall. 
Users are only allowed to access this server, once they are connected to the college's network (either direct or [VPN](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/remote-access/virtual-private-network-vpn/) access).


### Command line file transfer

#### Steps for setting up iRODS client in HPC CX1 

Please follow these steps to set up the iRODS clients in hpc for the first time

* Create directory `.irods` under home (e.g. `mkdir -p ~/.irods`)
* Create iRODS environment file `~.irods/irods_environment.json`
* Copy following configuration to the above mentioned file (replace USERNAME with your actual username) and validate file format using [JSONLint](https://jsonlint.com/)

##### Authentication Type: Standard
Use your IGF login password for setting up iRODS account in HPC, if the authentication type is Standard. You should be receiving the account credentials in a separate email from IGF.

<details>
<summary>Click to expand</summary>
<div style="background-color:#E8E8E8">
  <pre><code>
{
      "irods_host": "eliot.med.ic.ac.uk",
      "irods_port":1247,
      "irods_default_resource": "woolfResc",
      "irods_user_name": "YOUR_IGF_USERNAME",
      "irods_zone_name": "igfZone"
}

  </code></pre>
</div>
</details>
<p/>
##### Authentication Type: PAM
Use your Imperial login credential for setting up iRODS account in HPC, if the authentication type is PAM

<details>
<summary>Click to expand</summary>
<div style="background-color:#E8E8E8">
  <pre><code>
{
      "irods_host": "eliot.med.ic.ac.uk",
      "irods_port":1247,
      "irods_default_resource": "woolfResc",
      "irods_user_name": "YOUR_HPC_USERNAME",
      "irods_zone_name": "igfZone", 
      "irods_ssl_ca_certificate_file": "/apps/irods/certs/igf-chain.pem",
      "irods_ssl_ca_certificate_path": "/apps/irods/certs",
      "irods_ssl_verify_server": "cert",
      "irods_authentication_scheme": "PAM"
}

  </code></pre>
</div>
</details>
<p/>
#### Steps for command line transfer in HPC CX1

<p style="text-indent: 15px"><b>Step 1:</b> Load irods tool (e.g. <code>module load irods/4.2.0</code>)</p>
<p style="text-indent: 15px"><b>Step 2:</b> Set up your iRODS account using command <code>iinit</code> and specify your password</p>
<p style="text-indent: 15px"><b>Step 3:</b> Download data using commandline tool  <code>iget</code> (e.g. <code>iget -Pr /igfZone/home/USERNAME/PROJECT_NAME/PATH</code>)</p>
<p style="text-indent: 30px"><b>Step 3.1:</b> Download fastq data using commandline tool: <code>iget -Pr /igfZone/home/USERNAME/PROJECT_NAME/fastq</code></p>
<p style="text-indent: 30px"><b>Step 3.2:</b> Download analysis data using commandline tool: <code>iget -Pr /igfZone/home/USERNAME/PROJECT_NAME/analysis</code></p>

<div align="right"><a href="#table-of-contents">Go to Top</a></div>



## Access QC report pages

QC report pages for the raw and anlysed data files are accessible from our ftp site and accessible via the following url format `http://eliot.med.ic.ac.uk/report/project/PROJECTNAME`. You have to use the same login credentials for accessing these pages. For more details, please check [Project QC Report](qc_report_page.html) section of the help page.

You can access these pages from your mobile device if you are connected to wifi network [Imperial-WPA](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/wifi-and-networks/access-wifi/).

<div align="right"><a href="#table-of-contents">Go to Top</a></div>


## List of resources

* [iRODS](https://irods.org/)
* [iRODS server for genomic facility](http://eliot.med.ic.ac.uk:8080/idrop-web)
* [Illumina BaseSpace Sequence Hub](https://www.illumina.com/products/by-type/informatics-products/basespace-sequence-hub.html)

<div align="right"><a href="#table-of-contents">Go to Top</a></div>


## Change logs

* None
