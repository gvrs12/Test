#!/usr/bin/env groovy

library 'SCDM-Lib'

def String appname = 'helloworld'

node {
	env.CODEREPO = 'git'
	
	//Call the ant groovy file
	Build "${appname}"
}
