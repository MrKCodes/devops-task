package org.common

import hudson.model.*
import hudson.FilePath
import groovy.io.FileType
import groovy.transform.Field

import org.jenkinsci.plugins.workflow.cps.CpsScript

node {

    stage('Git Checkout'){
        git credentialsId: 'Github_kartikeya', url: 'https://github.com/kartikeyachauhan/devops-task.git'
    }
    stage('Build Docker Image'){
        sh 'docker build .'
    }
    stage('Build Stage'){
        sh '''
            mkdir dist
            cp *.py dist/
            cp requirements.txt dist/
            cp -R templates dist/
            echo "Starting  packaging `date` in `pwd`"
            zip -r dist.zip dist/* '''
    }
    stage('Archiving Artifact'){
        archiveArtifacts artifacts: '*.zip'
    }
    stage('Deploying to target'){

    }
    stage('Docker Registry'){
        sh 'docker push chauhankartikeya/smallcase-demo:latest'
    }

}