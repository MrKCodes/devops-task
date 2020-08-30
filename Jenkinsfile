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
        powershell label: '', script: 'docker build'
    }
    stage('Build Stage'){
        powershell label: '', script: 'Compress-Archive -LiteralPath templates, requirements.txt, application.py -CompressionLevel Optimal -DestinationPath application.zip'
    }
    stage('Archiving Artifact'){
        archiveArtifacts artifacts: '*.zip'
    }
    stage('Deploying to target'){

    }
    stage('Docker Registry'){

    }

}