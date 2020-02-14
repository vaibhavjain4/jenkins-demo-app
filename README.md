# External Jenkins (CI) Integration with OpenShift (CD) 

For this setup purpose we are using Jenkins ver. 2.204.2 (on Mac) & OpenShift v4.3 

Jenkins openshift client plugin : https://github.com/openshift/jenkins-client-plugin

<b>

Step 1 : Download Jenkins
  
Step 2 : Install Jenkins

Step 3 : Jenkins - OpenShift Client Plugin Installation 

Step 4 : Jenkins - Global Tool Configuration

Step 5 : Jenkins - Cluster Configuration

Step 6 : Jenkins openshift setup test - basic new ruby app pipeline

Step 7 : Jenkins (CI) & OpenShift (CD) Advanced Pipeline

</b>

## Step 1 : Download Jenkins

Download Jenkins from http://mirrors.jenkins-ci.org/osx-stable/?C=M;O=D 

For other platform , download jenkins from https://jenkins.io/download/

## Step 2 : Install Jenkins

Start the installer & follow on screen instructions to complete the Jenkins installation.

You will require "System Administrator" access to install Jenkins.

  <ul>
  <li> Copy the string from /Users/Shared/Jenkins/Home/secrets/initialAdminPassword and paste as administrator password during the installation when asked.</li>
  <br>
  <li> Choose option : "Install suggested plugins" </li>
  <br/>
  <li> Create an admin user as per your preference </li>
  </ul>
  
  
## Step 3 : Install Jenkins - OpenShift Client Plugin 

Enable OpenShift Client Jenkins Plugin : https://plugins.jenkins.io/openshift-client/

OpenShift Client Jenkins Plugin Package Download Link : https://updates.jenkins-ci.org/download/plugins/openshift-client/

Github Documentation Link : https://github.com/openshift/jenkins-client-plugin

Use following steps to download and configure plugin :

<ul>
  <li> Go to Manage Jenkins </li>
  <br>
  <li> Choose Manage Plugins </li>
  <br>
  <li> Click on Available Tab </li>
  <br>
  <li> Use Filter for search "Openshift" </li>
  <br>
  <li> Select "OpenShift Client", "OpenShift Login", "OpenShift Sync" Plugins </li>
  <br>
  <li> Download now and install after restart </li>
  <br>
  <li> Restart Jenkins </li>
</ul>




## Step 4 :



## Step 5 :



## Step 6:



## Step 7 :



## Reference URL's for additional information

<ul>
  <li> Github Documentation Link : https://github.com/openshift/jenkins-client-plugin </li>
<br>
  <li> Enable OpenShift Client Jenkins Plugin : https://plugins.jenkins.io/openshift-client/ </li>
<br>
  <li> OpenShift Client Jenkins Plugin Package Download Link : https://updates.jenkins-ci.org/download/plugins/openshift-client/ </li>
<br>
  <li> oc tool command download link : https://mirror.openshift.com/pub/openshift-v4/clients/oc/ </li>
<br>  
</ul>  
  
