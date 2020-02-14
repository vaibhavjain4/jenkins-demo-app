pipeline {
agent {
  node { label 'master' }
}
tools {
      oc 'oc'
  }
    stages {

                stage ('Create New Project') {
                    steps {
                    script {
                        openshift.withCluster( CLUSTER_NAME ) {
                                openshift.newProject( PROJECT_NAME )
                        }
                    }    
                    }
                }

                stage ('Deploy App') { 
                    steps {
                    script {
                        openshift.withCluster( CLUSTER_NAME ) {
                                openshift.withProject( PROJECT_NAME ) {
                                                echo "Hello from project ${openshift.project()} in cluster ${openshift.cluster()}"
                                                def created = openshift.newApp( 'https://github.com/openshift/ruby-hello-world' )
                                                echo "new-app created ${created.count()} objects named: ${created.names()}"
                                }
                        }       
                    }
                    }
                }
}
}
