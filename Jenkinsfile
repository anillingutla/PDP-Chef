#!/bin/groovy

import jenkins.model.*
import hudson.model.*
import hudson.EnvVars
import groovy.json.JsonSlurperClassic
import groovy.json.JsonBuilder
import groovy.json.JsonOutput
import java.net.URL

def environmentRepoCredentials = "chef-environments"
def chefAutomateCredentials = "chef-automate"
def githubCredentials = "ford-github"
def githubSshCredentials = "chef-automate-ssh"

//retrieve branch name
tokens = "${env.JOB_NAME}".tokenize('/')
repo = tokens[0]
branch = tokens[1]
echo 'repo/branch=' + repo +'/'+ branch

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
    
    GROOVY_PATH ='**/groovy'
  }
 
//  parameters {
//    string(description: 'Please select an environment to promote to', name: 'env', defaultValue: 'qa')
//    string(description: 'Please select a cookbook to promote', name: 'cookbook', defaultValue: 'ford_windows')
//  }
 

   stages {

       stage('\u2776 Workspace') {
        agent any
        steps {
              echo "${BRANCH_NAME} ${env.BRANCH_NAME}"
              script{
                    def manager = "my manager" // 
                    def workspace = manager.build.getEnvVars()["WORKSPACE"]
                    echo " workspace - ${env.WORKSPACE}"
              }
          
          }//steps
       }//stage
    
       stage('\u2777 External Groovy') {
        agent any
          steps {
           echo "Calling external Method groovy"
           script {
             // Load file from the current directory:
               def externalMethod = load("**/groovy/externalMethod.groovy")
               externalMethod.lookAtThis("Steve")

               echo "Calling external Call groovy"

               def externalCall = load("**/groovy/externalCall.groovy")
               externalCall("Steve")
           }
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
}//pipeline
