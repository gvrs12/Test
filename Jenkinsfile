
def workspace;
node ('Node1') {

    stage ('Checkout')
    {
        git credentialsId: '77f20f61-377f-4700-b712-6e5f0f5a0bff', url: 'git@github.com:gvrs12/Test.git' 
        
        echo "Checkout is scuccessful"
    }
    workspace = pwd()
    echo "${workspace}"
    
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
    
    stage ('Upload_to_Artifactory') {
        
        if (isUnix()){
            sh 'curl -uvenkat:AP5b85qJznNckJRKaNjWVSAjmuH -T target/helloworld.war "http://192.168.1.162:8081/artifactory/Test/helloworld.war"'
            echo "Uploaded to Artifactory"
        } else {
            bat 'curl -uvenkat:AP5b85qJznNckJRKaNjWVSAjmuH -T target/helloworld.war "http://192.168.1.162:8081/artifactory/Test/helloworld.war"'
            echo "Uploaded to Artifactory"
        }
    }
}
