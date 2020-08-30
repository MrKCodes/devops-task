package org.common

import hudson.model.*
import hudson.FilePath
import groovy.io.FileType
import groovy.transform.Field

import org.jenkinsci.plugins.workflow.cps.CpsScript

pipeline {
    environment {
        registry = "chauhankartikeya/smallcase-demo"
        registryCredential = 'DockerHub_kartikeya'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Clean Workspace'){
             steps {
                sh '''
                    rm -rf dist
                    rm dist.zip
                    '''
             }
        }
        stage('Cloning our Git') {
            steps {
                git credentialsId: 'Github_kartikeya', url: 'https://github.com/kartikeyachauhan/devops-task.git'
            }
        }
        stage('Building Docker image') {
            steps{
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Build Stage'){
            steps {
                sh '''
                    mkdir dist
                    cp *.py dist/
                    cp requirements.txt dist/
                    cp -R templates dist/
                    echo "Starting  packaging `date` in `pwd`"
                    zip -r dist.zip dist/* '''
            }
        }
        stage('Archiving Artifact'){
            steps {
                archiveArtifacts artifacts: '*.zip'
            }
        }
        
        stage('Deploying to target'){
            steps{
                echo 'Deploying to AWS EC2 instance'
            }

        }
        stage('Push to Docker Hub') {
            steps{
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Cleaning Images') {
            steps{
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}