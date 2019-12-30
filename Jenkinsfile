#!/usr/bin/env groovy

library 'SCDM-Lib'

def String appname = 'Helloworld'
def String buildtype = 'mvn'


node ('Node1') {
	env.CODEREPO = 'git'
	
	//Call the ant groovy file
	builder "${appname}", "${buildtype}"
}