#!/usr/bin/env groovy


pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS_ID = credentials('dockerhub')
        DOCKER_IMAGE_NAME = 'ibrahimabdelmonaem/app'
    }

    stages {
        stage('checkout code') {
            steps {
                git url: 'https://github.com/Ibrahim-Abdelmonaem/test.git', branch: 'main'
            }

        }
    stage('Build Docker Image') {
            steps {
                script {
                    dir('app') {
                    sh 'pwd'
                    sh "sudo docker build -t $DOCKER_IMAGE_NAME:latest ."

                    }
                }
            }
        }

        
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
                        //EC2_PUBLIC_IP = sh(
                            //script: "terraform output ec2_public_ip",
                            //returnStdout: true
                        //).trim()
                    }
                }
            }
        }
            }
        }
