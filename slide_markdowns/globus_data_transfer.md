## Globus data transfer
NEXT_SLIDE
#### Table of contents
<div style="font-size:34px">
<ul>
<li><a href="#/2">What is Globus</a></li>
<li><a href="#/4">How to login to Globus account</a></li>
  <ul>
    <li><a href="#/5">Login to Globus - for Imperial College users</a></li>
    <li><a href="#/6">Login to Globus - for external users</a></li>
  </ul>
<li><a href="#/7">How to transfer data</a></li>
  <ul>
    <li><a href="#/8">Transfer data to your computing cluster</a></li>
    <li><a href="#/10">Transfer data to your Personal Computer using Globus</a></li>
  </ul>
<li><a href="#/11">What information are needed for Globus data transfer</a></li>
<li><a href="#/12">How do you find shared data using Globus web app</a></li>
<li><a href="#/13">Globus transfer process</a></li>
</ul>
</div>
NEXT_SLIDE
### What is Globus
<p align="left">"Globus is a non-profit service for secure, reliable research data management. With Globus, subscribers can move, share, & discover data via a single interface - whether your files live on a supercomputer, lab cluster, tape archive, public cloud or your laptop, you can manage this data from anywhere, using your existing identities, via just a web browser."</p>
<ul>
<li>Visit <a href="https://www.globus.org">https://www.globus.org</a></li>
</ul>
NEXT_SLIDE
### What is Globus - video
<iframe src="https://player.vimeo.com/video/437243813?title=0&byline=0&portrait=0" width="560px" height="315px" frameborder="1"></iframe>
NEXT_SLIDE
### How to login to Globus account
* [For Imperial College users](#/5)
* [For external users](#/6)
NEXT_SLIDE
#### Login to Globus - for Imperial College users
<p align="left">Check the following Wiki page maintained by Imperial College's RCS team</p>
<ul>
<li><a href="https://wiki.imperial.ac.uk/display/HPC/Globus">Transferring data to other sites with Globus</a></li>
</ul>
NEXT_SLIDE
#### Login to Globus - for external users
<ul>
<li>Option 1: Use your existing organizational login</li>
<li>Option 2: Use Google or ORCID id</li>
</ul>
<p/>
<img src="../slide_images/globus_use_your_existing_login.png" style="border:1px solid black" width="600px" height="315px">
NEXT_SLIDE
### How to transfer data
* [Transfer data to your computing cluster](#/8)
* [Transfer data to your Personal Computer using Globus](#/10)
NEXT_SLIDE
#### Transfer data to your computing cluster
<p align="left">Check the following links:
<ul>
<li>For Imperial College users: <a href="https://wiki.imperial.ac.uk/display/HPC/Globus">Transferring data to other sites with Globus</a></li>
<li>For external users: <a href="https://docs.globus.org/how-to/get-started/">How To Log In and Transfer Files with Globus</a></li>
</ul>
</p>
NEXT_SLIDE
#### Transfer data to your computing cluster - docs
<iframe src="https://docs.globus.org/how-to/get-started/" width="600px" height="315px" frameborder="1"></iframe>
NEXT_SLIDE
#### Transfer data to your Personal Computer
<div style="font-size:32px">
<p align="left">Use <b>Globus Connect Personal</b> for transferring data to your own computer. This creates a Globus endpoint on your personal computer, allowing you to move data using the Globus web app</p>
<p align="left">Get Globus Connect Personal for:</p>
<ul>
<li><a href="https://docs.globus.org/how-to/globus-connect-personal-mac">Mac OS X</a></li>
<li><a href="https://docs.globus.org/how-to/globus-connect-personal-windows">Windows</a></li>
<li><a href="https://docs.globus.org/how-to/globus-connect-personal-linux">Linux</a></li>
</ul>
</div>

```bash
## Example commandline for Globus Connect Personal on Linux

./globusconnect -start -restrict-paths rw/path/globus_download

```
NEXT_SLIDE
### What information are needed for Globus data transfer
<div>
<p align="left">We need following information for each Globus transfer</p>
<ul>
<li>For Imperial College users - your college username</li>
  <ul>
    <li>For e.g., <span style="color:green">username@ic.ac.uk</span> (not <span style="color:crimson">your.name@imperial.ac.uk</span> or <span style="color:crimson">u.name@ic.ac.uk</span>)</li>
  </ul>
<li>For External users - your <span style="color:green">Globus username</span></li>
</ul>
</div>
NEXT_SLIDE
### How do you find shared data using Globus web app
<div>
<ul>
<li><a href="https://docs.globus.org/faq/transfer-sharing/">https://docs.globus.org/faq/transfer-sharing/</a></li>
</ul>
<iframe src="https://docs.globus.org/faq/transfer-sharing/" width="600px" height="315px" frameborder="1"></iframe>
</div>
NEXT_SLIDE
#### Globus transfer process
<div align="left" style="font-size:30px">
<ol>
<li>We will create a new Globus collection after receiving the user's request and copy existing data to it</li>
<li>The new collection will be shared with the user's Globus account (i.e., email id or username). We can add more than one user to the same Globus collection</li>
<li>User needs to follow <a href="https://www.globus.org/">Globus</a> or <a href="https://wiki.imperial.ac.uk/display/HPC/Globus">Imperial College's documentation</a> and transfer data (to any preferred storage location)</li>
<li>Files from any new sequencing run or analysis will get added to the existing Globus collection directory</li>
<li>User needs to transfer any new batch of data separately after receiving email notification from us</li>
<li>Each file on the Globus collection directory will be removed after 30 days of file creation</li>
<li>We may remove old Globus collections without any prior notice, once all the files are removed from the collection directory</li>
</ol>