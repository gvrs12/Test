#!/usr/bin/env groovy

import java.util.Date.*
import java.util.Calender.*

library 'SCDM-Lib'

def String appname = 'helloworld'

node {
	def jobstatus = "Pass"
	env.APPNAME="${appname}"
	echo "${env.APPNAME}"
	def build = BUILD_TIMESTAMP

	def dateFormat = "yyyy-MM-dd HH:mm:ss.SSS z"
	Date start = Calender.getInstance().getTime()
	echo "Start Time: ${start.format(dateFormat)}"

	try {
		stage ('Checkout') {
			//checkout scm
			if (CODEREPO == "git")
			{
			final scmVars = checkout(scm)
			env.branchname = "${scmVars.GIT_BRANCH}"
			echo "branchname is: ${scmVars.GIT_BRANCH}"

			def String GIT_URL = "${scmVars.GIT_URL}"
			echo "GIT URL is: ${scmVars.GIT_URL}"
			env.SCM_URL = "${GIT_URL}"
			}
		}

		stage ('Build')
		{
		if (isUnix()){
		    sh 'mvn -DskipTests clean package'
		    echo "Build is successful"
		} else {
		    bat 'mvn -DskipTests clean package'
		    echo "Build is successful"
		}
		}

		stage ('Upload') {
			def server = Artifactory.server "JFrog"
			def buildInfo = Artifactory.newBuildInfo()
			buildInfo.env.capture = true
			buildInfo.env.collect()

			def uploadSpec = """{
		    "files": [
			{
			    "target": "Test/${appname}/${appname}_${env.BRANCH_NAME}_${build}.war",
			    "pattern": "target/${appname}.war"
			}
		    ]
		}"""
		server.upload(uploadSpec)
			server.upload spec: uploadSpec, buildInfo: buildInfo
		}
	}
	catch (e) {
		// If there was an exception thrown, the build failed
		jobstatus = "Fail"
		//trace e
		currentBuild.result = "FAILURE"
	}
	finally {
		stage('clean'){
			echo "Cleaning our workspace directory ${WORKSPACE} "
			cleanWs()
		}
	}

}
