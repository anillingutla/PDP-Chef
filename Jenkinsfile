#!/bin/groovy
 
def environmentRepoCredentials = "chef-environments"
def chefAutomateCredentials = "chef-automate"
def githubCredentials = "ford-github"
def githubSshCredentials = "chef-automate-ssh"

//retrieve branch name
tokens = "${env.JOB_NAME}".tokenize('/')
org = tokens[0]
repo = tokens[1]
branch = tokens[2]
echo 'account-org/repo/branch=' + org +'/'+ repo +'/'+ branch

//get workspace being worked on
def workspace = pwd()
echo "\u2600 workspace=${workspace}"

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
 
  parameters {
    string(description: 'Please select an environment to promote to', name: 'env', defaultValue: 'qa')
    string(description: 'Please select a cookbook to promote', name: 'cookbook', defaultValue: 'ford_windows')
  }
 

   stages {

       stage('\u2775 External Groovy') {
          steps {
           echo "Calling external Method groovy"
           node {
             // Load file from the current directory:
               def externalMethod = load("externalMethod.groovy")
               externalMethod.lookAtThis("Steve")

               echo "Calling external Call groovy"

               def externalCall = load("externalCall.groovy")
               externalCall("Steve")
           }
          }
        }

       stage('\u2776 clone') {
          steps {
            echo "Cloning ${GIT_URL}"
 //              dir("ford_rdc_environments") {
 //                sshagent([githubSshCredentials]) {
 //                  git(url: "${GIT_PROTOCOL}${GIT_URL}", credentialsId: githubSshCredentials)
 //                }
 //              }
          }
        }

        stage('\u2777 modify') {
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
}//pipeline
