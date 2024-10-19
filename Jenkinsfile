#!/usr/bin/env groovy


pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS_ID = credentials('dockerhub')
        DOCKER_IMAGE_NAME = 'ibrahimabdelmonaem/app:latest'
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
                    sh "sudo docker build -t $DOCKER_IMAGE_NAME ."
                    sh 'pwd'
                    sh 'echo build ok'
                    }
                }
            }
        }

    stage('Push to Docker hub') {
            steps {
                script {
                    sh "echo hello1"
                    sh "echo hello2"
                    sh "echo login not ok"
                    sh 'sudo docker login -u $DOCKERHUB_CREDENTIALS_ID_USR -p $DOCKERHUB_CREDENTIALS_ID_PSW'
                    sh "sudo docker images"
                    sh "echo login ok"
                    sh "sudo docker push $DOCKER_IMAGE_NAME"
                    sh "echo $DOCKER_IMAGE_NAME"
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
