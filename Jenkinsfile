#!/usr/bin/env groovy

library 'SCDM-Lib'

def String appname = 'helloworld'

node ('Node1') {
	env.CODEREPO = 'git'
	
	//Call the ant groovy file
	Build "${appname}"
}
