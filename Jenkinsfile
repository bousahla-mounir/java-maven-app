def gv

pipeline {
    agent any
    /*parameters {
        choice(name: 'VERSION', choices: ['1.1.0','1.2.2','1.3.4'], description: '')
        booleanParam(name: 'executeTests', defaultValue: true, description: '')
    }*/
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
                    echo "build jar from $BRANCH_NAME"
                }
            }
        }
        stage("build image") {
            steps {
                script {
                    //gv.buildImage()
                    echo "build image $BRANCH_NAME"
                }
            }
        }
        stage("deploy") {
            /*input {
                message "Select the environment to deploy to"
                ok "Done"
                parameters {
                    choice(name: 'ONE', choices: ['dev', 'staging', 'prod'], description: '')
                    choice(name: 'TWO', choices: ['dev', 'staging', 'prod'], description: '')
                }
            }*/
            steps {
                script {
                    /*echo "deploying the application ..."
                    echo "deploying version ${params.VERSION}"
                    echo "Deploying to ${ONE}"
                    echo "Deploying to ${TWO}"*/
                    //gv.deployApp()
                    echo "deploy $BRANCH_NAME"
                }
            }
        }
    }   
}