package org.common

import hudson.model.*
import hudson.FilePath
import groovy.io.FileType
import groovy.transform.Field

import org.jenkinsci.plugins.workflow.cps.CpsScript

def build = "dist" + BUILD_NUMBER + '.zip'
 
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
                if [ -d dist ] ; then
                     rm -rf dist
                fi
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
                sh """
                    mkdir dist
                    cp *.py dist/
                    cp requirements.txt dist/
                    cp -R templates dist/
                    echo "Starting  packaging `date` in `pwd`"
                    zip -r "$build" dist/* """
            }
        }
        stage('Archiving Artifact'){
            steps {
                archiveArtifacts artifacts: '*.zip', fingerprint: true
            }
        }
        
        stage('Deploying to target'){
            steps{
                echo 'Deploying to AWS EC2 instance'
                
            }

        }
        stage('Push Artifact/ Docker image') {
            steps {
                parallel(
                'Upload to S3 Bucket': {
                    echo "S3 Bucket"
                    sh "mvn s3-upload"
                    // s3Upload file:"dist$BUILD_NUMBER.zip", bucket:'smallcase-artifacts', path:"s3://smallcase-artifacts/develop/dist$BUILD_NUMBER.zip"
                    
                },
                'Push to Docker Hub': {
                    script {
                        docker.withRegistry('', registryCredential) {
                            dockerImage.push()
                    }
                }
                }
                )
            }
            }
        // stage('Push to Docker Hub') {
        //     steps{
        //         script {
        //             docker.withRegistry('', registryCredential) {
        //                 dockerImage.push()
        //             }
        //         }
        //     }
        // }
        stage('Cleaning Images') {
            steps{
                sh """
                    docker rmi $registry:$BUILD_NUMBER
                    rm -rf *
                    """
            }
        }
    }
}