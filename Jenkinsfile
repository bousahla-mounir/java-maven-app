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
        stage('increment version') {
            steps {
                script {
                    echo 'incrementing app version ...'
                    sh  'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'
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
        stage("commit version update") {
            steps {
                script {
                    withCredentials([script.usernamePassword(credentialsId: 'github-credential', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh 'git config --global user.email "jenkins@example.com"'
                        sh 'git config --global user.name "jenkins"'

                        sh 'git status'
                        sh 'git branch'
                        sh 'git config --list'

                        sh "git remote set-url origin https://${USER}:${PASS}@github.com/bousahla-mounir/java-maven-app.git"
                        sh 'git add .'
                        sh 'git commit -m "ci: version bump"'
                        sh 'git push origin HEAD:jenkins-shared-lib'
                    }
                }
            }
        }
    }   
}