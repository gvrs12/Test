node ('Node1') {
    def build = BUILD_TIMESTAMP
    //buildTimeStamp = buildTimeStamp.split(" IST")
    stage ('Checkout')
    {
        //git credentialsId: '77f20f61-377f-4700-b712-6e5f0f5a0bff', url: 'git@github.com:gvrs12/Test.git' 
        
        //echo "Checkout is scuccessful"
	checkout scm
	final scmVars = checkout(scm)
	env.branchname = "${scmVars.GIT_BRANCH}"
	echo "branchname is: ${scmVars.GIT_BRANCH}"

	def String GIT_URL = "${scmVars.GIT_URL}"
	echo "GIT URL is: ${scmVars.GIT_URL}"
	def String[] URL_git = GIT_URL.split('//')
	echo "URL_git is: ${URL_git}"
	def String url_noHttps = URL_git[1]
	env.GITURL_nohttps = "${url_noHttps}"
	env.SCM_URL = "${GIT_URL}"
	echo "SCM_URL is: ${GIT_URL}"
    }

    stage ('Build')
    {
        if (isUnix()){
            sh 'mvn clean package'
            echo "Build is successful"
        } else {
            bat 'mvn clean package'
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
                    "target": "Test/helloworld_${env.BRANCH_NAME}_${build}.war",
                    "pattern": "target/helloworld.war"
                }
            ]
        }"""
        server.upload(uploadSpec)
		server.upload spec: uploadSpec, buildInfo: buildInfo
	}
    
    stage ('Download') {
		def server = Artifactory.server "JFrog"
		
		def downloadSpec = """{
         "files": [
          {
              "pattern": "Test/*${build}.war",
              "target": "download/helloworld.war"
            }
         ]
        }"""
        server.download spec: downloadSpec
	}
	
	stage ('Deploy')
	{
	    deploy adapters: [tomcat9(credentialsId: '83d19875-d5d4-4533-9f97-f31cc4039cb2', path: '', url: 'http://192.168.1.152:8082')], contextPath: null, onFailure: false, war: 'download/*.war'
	}
}
