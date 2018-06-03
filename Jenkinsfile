#!/usr/bin/env groovy

@Library('my-shared-library@dev') _

import var.*
import jenkins.model.*
import hudson.model.*
import jenkins.*
import hudson.*

def environmentRepoCredentials = "chef-environments"
def chefAutomateCredentials = "chef-automate"
def githubCredentials = "ford-github"
def githubSshCredentials = "chef-automate-ssh"

pipeline {
  agent any
 
  environment {
    // Chef Automate information
    AUTOMATE_URL = 'https://chef-val-wfl.hplab1.ford.com'
    AUTOMATE_ENTERPRISE = 'ford'
    AUTOMATE_ORG = 'POC'
    AUTOMATE_PROJECT = 'ford_rdc_environments'
    AUTOMATE_PIPELINE = 'master'
 
    // GitHub Configuration
    GIT_URL = 'git@github.ford.com/JBODNAR/ford_rdc_environments.git'
    GIT_PROTOCOL = 'ssh://'
    GIT_COMMAND = 'delivery review'
    
    GROOVY_PATH ='groovy'
    MAX_BUILDS ='10'
  }
 
 /* Only keep the 10 most recent builds. */
  options {
      buildDiscarder(logRotator(numToKeepStr:'5'))
      disableConcurrentBuilds()
  }

 
//  parameters {
//    string(description: 'Please select an environment to promote to', name: 'env', defaultValue: 'qa')
//    string(description: 'Please select a cookbook to promote', name: 'cookbook', defaultValue: 'ford_windows')
//  }

   stages {
            
       stage('\u2777 External Groovy') {
        agent any
          steps {
           script {
           
                    // Load the file 'externalMethod.groovy' from the current directory, into a variable called "externalMethod".
                   // def externalMethod = load("$GROOVY_PATH/externalMethod.groovy")
                    echo "Calling external Method groovy"
                    // Call the method we defined in externalMethod.
                    externalMethod("Steve")
                    echo " Calling external call groovy"
                    // Now load 'externalCall.groovy'.
                    //def externalCall = load("$GROOVY_PATH/externalCall.groovy")
                    // We can just run it with "externalCall(...)" since it has a call method.
                    externalCall("Steve")

            }//script
          }//steps
        }//stage

       stage('\u2778 clone') {
          steps {
            echo "Cloning ${GIT_URL}"
 //              dir("ford_rdc_environments") {
 //                sshagent([githubSshCredentials]) {
 //                  git(url: "${GIT_PROTOCOL}${GIT_URL}", credentialsId: githubSshCredentials)
 //                }
 //              }
          }//steps
        }//stage

        stage('\u2779 modify') {
          steps {
            echo "Modifying ${params.cookbook} in environments/${params.env}.json"
            sh 'ruby --version'
 //             withCredentials([usernamePassword(credentialsId: chefAutomateCredentials, usernameVariable: 'AUTOMATE_USER', passwordVariable: 'AUTOMATE_PASSWORD')]) {
 //                withCredentials([usernamePassword(credentialsId: githubCredentials, usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD' )]) {
 //                  sshagent([githubSshCredentials]) {
 //                    sh "ruby main.rb ${params.env} ${params.cookbook} ford_rdc_environments"
 //                  }//sshagent
 //               }//withcredentials
 //              }//withcredentials
          }//steps
        }//stage
   }//stages
   post {
    always {
      deleteDir() //cleanup directory
    }
    success {
      mail to:"admin@admin.com", subject:"SUCCESS: ${currentBuild.fullDisplayName}", body: "Build Successful."
    }
    failure {
      mail to:"admin@admin.com", subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Build Failed."
    }
  } 
  
}//pipeline
