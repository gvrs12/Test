
def workspace;
node ('Node1') {
    def build = BUILD_NUMBER
    //buildTimeStamp = buildTimeStamp.split(" IST")
    stage ('Checkout')
    {
        git credentialsId: '77f20f61-377f-4700-b712-6e5f0f5a0bff', url: 'git@github.com:gvrs12/Test.git' 
        
        echo "Checkout is scuccessful"
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
                    "target": "Test/helloworld_${build}.war",
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
