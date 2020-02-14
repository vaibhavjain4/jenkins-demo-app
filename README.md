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

      Step 7a : Openshift configurations : build skeleton
      Step 7b : Test configurations : manually using oc commands
      Step 7c : Jenkins : Configure openshift cluster details and run pipeline
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

#### Use following steps to download and configure plugin :

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
  <br>
  <li> Go to Installed Tab (under managed plugins) & confirm openshift plugins are checked </li>
</ul>

## Step 4 : Configure Jenkins Global Tool Configuration

This step is to configure tools such as java, maven and openshift client (oc) to be used by pipelines. Tools name given under these settings will be called under pipeline tool section.

Go to Manage Jenkins -> Global Tool Configuration


<h4>OpenShift Client Tools</h4>
<ul>
  <li>Click on "Add OpenShift Client Tools"</li>
      <ul>
      <li> Name : oc </li>
      <li> Install automatically : checked </li>
      <li> Click on "Add OpenShift Client Tools </li>  
      <li> Choose "Extract *.zip/*.tar.gz"</li>
      <li> Label : Leave empty</li>
      <li> Download URL for binary archive : https://mirror.openshift.com/pub/openshift-v4/clients/oc/latest/macosx/oc.tar.gz  ( !! Important - Use the oc url as per your architecture !!)</li>
      <li> Subdirectory of extracted archive : Leave empty</li>  
      <li> Click Apply </li>  
      </ul>  
</ul> 
<h4> JDK Installations </h4>
<ul>
  <li> Click on "Add JDK" </li>
    <ul>
      <li>Name : jdk8</li>
      <li> Install Automatically : uncheck </li>
      <li> JAVA_HOME : /Library/Java/JavaVirtualMachines/jdk1.8.0_112.jdk/Contents/Home/ (!! Important - Change this to your installation directory location !!) </li>
      <li> Click Apply </li>
    </ul>
  </ul>
  <h4> Maven Installations </h4>
  <ul> 
    <li> Click on "Add Maven" </li>
      <ul>
        <li> Name : maven-3.6.3 </li>
        <li> Install automatically : checked </li>
        <li> Install from Apache Version : 3.6.3 </li>
        <li> Click Apply & Save </li>
      </ul>  
  </ul>

## Step 5 : Jenkins Configurations : Configure OpenShift Cluster Details

Go to Manage Jenkins -> Configure System

<h4> OpenShift Client Plugin </h4>
  <ul> 
    <li> Cluster Configurations : Click on "Add OpenShift Cluster" </li>
    <li> Cluster Name : openshift-cluster</li>
    <li> API Server URL : https://api.cluster-8a3a.sandbox956.opentlc.com:6443 (!! Important - Change this to your url !!)</li>
    <li> Disable TLS Verify : checked</li>
    <li> Credentials : Click on "Add"</li>
    <li> Kind : "OpenShift Token for OpenShift Client Plugin"</li>
    <li> Token : Paste your token here (!! Hint - Get token from $oc whoami -t !!)</li>
    <li> ID : my-user-token</li>
    <li> Click "Add"</li>
    <li> Credentials : Choose from drop down "my-user-token"</li>
    <li> Click on Apply & Save</li>
  </ul>


## Step 6: Jenkins - OpenShift Cluster Configuration Test - New Ruby App Pipeline

Let's create a new pipeline in Jenkins to test above done configurations. This pipeline will use parameters to capture namespace and openshift cluster name (as configured above earlier). 

A new project/namespace will be created in openshift (so make sure it doesn't exist) and new ruby app will be created using S2I method (code pull from github).

Open Jenkins -> New Item

<h4> Create New Pipeline </h4>
<ul>
  <li> Enter an item name : OpenShift Setup Test Pipeline </li>
  <li> Select "Pipeline" & Click "OK" </li>
  <li> Under General : Check / Select "This project is parameterized" </li>
  <li> Click on "Add Parameter" & Choose "String Parameter" </li>
      <ul>
        <li> Name : PROJECT_NAME</li>
      </ul>  
  <li> Click on "Add Parameter" & Choose "String Parameter" </li>
      <ul>
        <li> Name : CLUSTER_NAME</li>
        <li> Default Value : openshift-cluster (!! Important - This should match with earlier defined cluster name !!)
      </ul>
  <li> Pipeline </li>
      <ul>
        <li> Script : Paste content from https://github.com/vaibhavjain4/jenkins-demo-app/blob/master/JenkinsOpenShiftConfigurationTestPipeline.txt </li>
      </ul>
  <li> Click on Apply & Save </li>
</ul>

<h4> Run the pipeline </h4>
<ul>  
  <li> Click on Build with Parameters </li>
  <li> PROJECT_NAME : jenkins-setup-test </li>
  <li> CLUSTER_NAME : openshift-cluster </li>
  <li> Click on "Build" </li>
</ul>  

Once build has run successful, go and check openshift for new project and application to confirm.

## Step 7 : Advanced Pipeline - Build + Test In External Jenkins & Deploy + Promote In OpenShift

For this pipeline we will use following usecase :

<img src="https://github.com/vaibhavjain4/jenkins-demo-app/raw/master/Pipeline%20Overview.png" width="800px" />

  ### Step 7a : Openshift configurations : build skeleton
  
  Use following commands to build openshift skeleton for Jenkins Pipeline to invoke objects. (Note : This can also be done using pipeline but we are using manual method to explain the concepts and artifacts required.
  
  Download and configure oc utility (link given in references section below ) on your local machine/workstation/laptop to execute following command :
  
    oc login https://api.cluster-8a3a.sandbox956.opentlc.com:6443
    
    <br>
    <b>
    #### Create Two Namespaces ####
    </b>
    oc new-project my-project-dev

    oc new-project my-project-stage
    <br>
    <b>
    #### Create empty build, application, service & route in development environment (project). Disable automatic triggers. ####
    </b>
    oc new-build --name=microservice-app --image-stream=java:8 --binary=true -n my-project-dev

    oc new-app --name=microservice-app microservice-app:latest --allow-missing-images -l app=microservice-app -n my-project-dev

    oc rollout cancel dc/microservice-app -n my-project-dev

    oc set triggers dc/microservice-app --from-config --remove -n my-project-dev

    oc set triggers dc -l app=microservice-app --containers=microservice-app --from-image=microservice-app:latest --manual -n my-project-dev

    oc expose dc microservice-app --port=8080 -n my-project-dev

    oc expose svc microservice-app -n my-project-dev
    <br>
    <b>
    #### Create empty application, service & route in stage environment (project). Disable automatic triggers. ####
    </b>
    oc new-app --name=microservice-app microservice-app:stage --allow-missing-images -l app=microservice-app -n my-project-stage

    oc rollout cancel dc/microservice-app -n my-project-stage

    oc set triggers dc/microservice-app --from-config --remove -n my-project-stage

    oc set triggers dc -l app=microservice-app --containers=microservice-app --from-image=microservice-app:stage --manual -n my-project-stage

    oc create service clusterip microservice-app --tcp=8080:8080 -n my-project-stage

    oc expose svc microservice-app -n my-project-stage
    <br>
    <b>
    #### Create Service Account in Development Project. This account will be used from Jenkins to invoke build etc ####
    </b>
    oc create sa jenkins -n my-project-dev
    <br>
    <b>
    #### Give permissions to service account for editing and deploy applications into dev and stage namespaces/projects. ####
    </b>
    oc policy add-role-to-user edit system:serviceaccount:my-project-dev:jenkins -n my-project-dev

    oc policy add-role-to-user edit system:serviceaccount:my-project-dev:jenkins -n my-project-stage

    oc policy add-role-to-group system:image-puller system:serviceaccounts:my-project-dev -n my-project-dev

    oc policy add-role-to-group system:image-puller system:serviceaccounts:my-project-stage -n my-project-dev
    

  ### Step 7b : Test configurations : manually using oc commands
  
  
  ### Step 7c : Jenkins : Configure openshift cluster details and run pipeline



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
  
