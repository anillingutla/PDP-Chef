#!/usr/bin/env groovy

@Library('my-shared-library@dev') _

import jenkins.model.*
import jenkins.*

def environmentRepoCredentials = "chef-environments"
def chefAutomateCredentials = "chef-automate"
def githubCredentials = "ford-github"
def githubSshCredentials = "chef-automate-ssh"
def cookbooksChanged = false
  
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
  }

  /* Only keep the 10 most recent builds. */
  options {
      buildDiscarder(logRotator(numToKeepStr:'5'))
      disableConcurrentBuilds()
      timeout(time: 15, unit: 'MINUTES')
  }
 
//  parameters {
//    string(description: 'Please select an environment to promote to', name: 'env', defaultValue: 'qa')
//    string(description: 'Please select a cookbook to promote', name: 'cookbook', defaultValue: 'ford_windows')
//  }

   stages {

      stage('\u2776 Check for changes in Repo') {
          agent {
            label 'master'
          }
          steps {
                echo "Build triggered via branch: ${env.NODE_NAME}"
    
                //branch name from Jenkins environment variables
                echo "My branch is: ${env.BRANCH_NAME}"
                echo "My GIT_URL is: ${env.GIT_URL}"
      
                script {
                  if (env.BRANCH_NAME != "master") {
                        log.info ("Checking Master for Changes")
                        sh "git config --add remote.origin.fetch +refs/heads/master:refs/remotes/origin/master"
                        sh "git fetch --no-tags"
                        echo " after shell script "

                        List<String> sourceChanged = sh(returnStdout: true, script: "git diff --name-only origin/master..origin/${env.NODE_NAME}").split()

                        for (int i = 0; i < sourceChanged.size(); i++) {
                            if (sourceChanged[i].contains("cookbook_list.yml")) {
                                cookbooksChanged = true
                            }
                        }
                    } //if master

                    if (cookbooksChanged) {
                          log.info ("changes Identified")
                    }else{
                          log.info ("NO changes Identified")
                    }
               } //script   
            }//steps
        }//stage    
     
       stage('\u2777 External Groovy') {
          agent {
            label 'master'
          }
          steps {
            
                    deleteDir()
                    //library 'my-shared-library@dev'
                    echo "Calling external Method groovy"
                    externalMethod("Steve")
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
