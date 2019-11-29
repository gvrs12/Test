def workspace;
node ('Windows') {
    stage ('Checkout')
    {
        git credentialsId: '77f20f61-377f-4700-b712-6e5f0f5a0bff', url: 'git@github.com:gvrs12/Test.git' 
        workspace = pwd()
        echo "${workspace}"
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
}
