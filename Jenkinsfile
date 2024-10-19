#!/usr/bin/env groovy


pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    environment {
        IMAGE_NAME = 'nanajanashia/demo-app:java-maven-2.0'
    }

    stages {
        
        stage('provision server') {
            environment {
                AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
                AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
                TF_VAR_env_prefix = 'test'
            }
            steps {
                script {
                    dir('terraform') {
                        sh "terraform init -migrate-state"
                        sh "terraform apply --auto-approve"
                    }
                }
            }
        }

    stage('build app') {
            steps {
               script {
                  echo 'building application jar...'
                  buildJar()
               }
            }
        }


        
            }
        }
