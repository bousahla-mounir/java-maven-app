#!/usr/bin/var groovy

//@Library('jenkins-shared-library')   // if we work with system=>Global Trusted Pipeline Libraries
library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
    [
        $class: 'GitSCMSource',
        remote: 'https://github.com/bousahla-mounir/jenkins-shared-library.git',
        credentialsId: 'github-credential'
    ]
)
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
                    def IMAGE_NAME = "adabachir/demo-repo:jma-9.$BUILD_NUMBER"
                    buildImage "$IMAGE_NAME"
                    dockerLogin()
                    dockerPush "$IMAGE_NAME"
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