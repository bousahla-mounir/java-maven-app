#!/usr/bin/var groovy

@Library('jenkins-shared-library')
def gv

pipeline {
    agent any
    tools {
        maven 'maven-3.9'
    }
    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("build jar") {
            steps {
                script {
                    //gv.buildJar()
                    //echo "build jar from $BRANCH_NAME"
                    buildJar()
                }
            }
        }
        stage("build and push image") {
            steps {
                script {
                    //gv.buildImage()
                    echo "build image $BRANCH_NAME"
                    buildImage 'adabachir/demo-repo:jma-8.3'
                    dockerLogin()
                    dockerPush 'adabachir/demo-repo:jma-8.3'
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    gv.deployApp()
                    echo "deploy $BRANCH_NAME"
                }
            }
        }
    }   
}