#!/usr/bin/env groovy


pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS_USER = credentials('dockerhub_usr')
        DOCKERHUB_CREDENTIALS_PSW = credentials('dockerhub_psw')
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
                        sh "sudo docker build -t $DOCKER_IMAGE_NAME ."
                        sh 'echo build ok' 
                    }
                }
            }
        }

    stage('Push to Docker hub') {
            steps {
                script {
                    sh "echo login not ok"
                    sh "sudo docker login -u $DOCKERHUB_CREDENTIALS_USER -p $DOCKERHUB_CREDENTIALS_PSW"
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
                        ec2_public_ip = sh "sudo terraform output server-ip"
                        EC2_PUBLIC_IP = sh(
                            script: "terraform output server-ip",
                            returnStdout: true
                        ).trim()
                        env.SERVER_IP = EC2_PUBLIC_IP
                        echo "fuck ip address ${env.SERVER_IP}"
                        sh 'echo "[server]" > hosts'
                        //sh 'echo ${env.SERVER_IP} >> hosts'

                        //sh "echo [server]" > hosts
                        //sh "echo $EC2_PUBLIC_IP" >> hosts
                        sh "pwd"
                        
                    }
                    //sh "echo $EC2_PUBLIC_IP >> hosts"
                }
            }
        }


        stage('Configurations with ansible') {
            steps {
                script {
                sh 'pwd'
                sh 'ls'
                sh 'ansible-playbook -i hosts deploy.yaml'
                }
            }
        }







            }
        }
