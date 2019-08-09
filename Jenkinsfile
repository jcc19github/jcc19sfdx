#!groovy
import groovy.json.JsonSlurperClassic
node {
    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def PACK_DIR="MyPackage${BUILD_NUMBER}"
    def SFDC_USERNAME
    def workspace = env.WORKSPACE

    def HUB_ORG=env.HUB_ORG_DH_SB
    def SFDC_HOST = env.SFDC_HOST_DH_SB
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH_SB
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH_SB

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
  
    stage('Clone Source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
    //Auth JWT FLow
    sfdx force:auth:jwt:grant
    
    stage('Source Conversion') {
        sfdx force:source:convert
    }
            
    stage('Validation') {
        sfdx force:mdapi:deploy with -c
    }

    stage('Deployment') {
        sfdx force:mdapi:deploy
    }
            
}