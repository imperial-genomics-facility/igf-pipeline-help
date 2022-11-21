## Accessing data files from IGF

#### Table of contents
* [Data Sharing via Globus](#/2)
* [Data upload to Research Data store (RDS)](#/4)
* [Basespace upload](#/5)
* [Data upload to iRODS server](#/6)
* [Custom data upload](#/11)
* [Understanding fastq output](#/12)
* [Validation of the fastq files](#/13)

#### Data Sharing via Globus
* [Globus](https://www.globus.org/) allowes the following options:
  * __Transfer__ large volumes of data between the [RDS](https://wiki.imperial.ac.uk/display/HPC/Transferring+data+to+and+from+the+RDS), your personal computer and Globus-accessible storage at other institutions
  * __Share__ [RDS](https://wiki.imperial.ac.uk/display/HPC/Transferring+data+to+and+from+the+RDS) project allocation data with selected third parties, without requiring them to have a College account (Globus identity required)

#### Data Sharing via Globus requirements
  * We will require your Colleges username (e.g., `username@ic.ac.uk`) for this mode of data sharing.
  * For more details, please have a look at Imperial College's guideline for Globus data transfer: [Transferring data to other sites with Globus](https://wiki.imperial.ac.uk/display/HPC/Globus)

#### Data upload to Research Data store (RDS)
* Connect to Imperial College network (direct LAN connection or set up [VPN](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/remote-access/virtual-private-network-vpn/))
* [Set-up RDS project allocation](https://imperial-genomics-facility.github.io/igf-pipeline-help/data_access.html#imperial-college-research-data-store-based-transfer)
* Add user '__igf__' to the allocation
* Send us the IGF project id and RDS path
* Remove user '__igf__' from allocation
* Validate fastq files

#### Basespace upload
* Fastq files can be shared via Basespace sequence hub (Illumina)
  * We use [Basespace London region](https://euw2.sh.basespace.illumina.com) for data upload and share
* Data can be download via [Basespace cli](https://developer.basespace.illumina.com/docs/content/documentation/cli/cli-overview) or via [browser](https://euw2.sh.basespace.illumina.com)
  * API server: <a style="font-size:35px"><code>https://api.euw2.sh.basespace.illumina.com</code></a>

#### iRODS based transfer - using command line
<span style="color:crimson"><b>Update:</b> We have stopped using iRODS server for data distribution from <b>October 2022</b>.</span>
* Connect to Imperial College network (direct LAN connection or set up [VPN](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/remote-access/virtual-private-network-vpn/))
* Set up iRODS on HPC cluster CX1
* Use iRODS command for transferring data

####  Create iRODS config file on HPC CX1
```bash
touch ~/.irods/irods_environment.json
```

#### Add iRODS config for authentication type - Standard
<span><p align="left">Add the following entries to <code><a style="font-size:20px">~/.irods/irods_environment.json</a></code></p></span>
<span><p align="left">Use your IGF login password for setting up iRODS account on HPC</p></span>
```json [1-7|5]
{
    "irods_host": "eliot.med.ic.ac.uk",
    "irods_port": 1247,
    "irods_default_resource": "woolfResc",
    "irods_user_name": "YOUR_IGF_USERNAME",
    "irods_zone_name": "igfZone"
}
```
<p style="font-size:25px">Don't forget to replace "YOUR_IGF_USERNAME"</p>

#### Add iRODS config for authentication type - PAM
<span><p align="left">Add the following entries to <code><a style="font-size:20px">~/.irods/irods_environment.json</a></code></p></span>
<span><p align="left">Use your Imperial login credential for setting up iRODS account in HPC</p></span>
```json [1-11|5]
{
    "irods_host": "eliot.med.ic.ac.uk",
    "irods_port": 1247,
    "irods_default_resource": "woolfResc",
    "irods_user_name": "YOUR_HPC_USERNAME",
    "irods_zone_name": "igfZone", 
    "irods_ssl_ca_certificate_file": "/apps/irods/certs/igf-chain.pem",
    "irods_ssl_ca_certificate_path": "/apps/irods/certs",
    "irods_ssl_verify_server": "cert",
    "irods_authentication_scheme": "PAM"
}
```
<p style="font-size:25px">Don't forget to replace "YOUR_HPC_USERNAME"</p>

#### Command line file transfer on HPC CX1
```bash [1-3|1,4-5|1,6-7]
module load irods/4.2.0
## configure iRODS account
iinit
## download fastq files
iget -r /igfZone/home/USERNAME/PROJECT_NAME/fastq
## or for downloading analysis files
iget -r /igfZone/home/USERNAME/PROJECT_NAME/analysis
```
<span><p align="left">Use following iRODS paths:</p></span>
*  __fastq__: To download all fastq files
*  __analysis__: To download all primary analysis files

#### Custom data upload
* Raw data can be shared via external FTP server
* QC data can be shared via [Box](box.com)

#### Understanding fastq output
<span><p align="left">Each lane level tar file (or lane level dir) has the following output files</p></span>
* Fastq reads
  * __R1__ and __R2__ (and __R3__) fastq files are read sequences
  * __I1__ and __I2__ fastq files are index sequences
* Demuliplexing html report
* Manifest file containing the md5 checksum of the fastq files
* Samplesheet file (for the lane)

#### Validation of the fastq files
* Locate __file_manifest.csv__ file for each lane
* Run the following command for md5sum check (for fastq files)
```bash
grep -v file *file_manifest.csv|\
awk '{ print $2 " " $1 }'|\
md5sum -c
```
