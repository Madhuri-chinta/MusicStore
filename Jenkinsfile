pipeline {
    agent { label 'UBUNTU_NODE1'}
    //triggers { cron ('*/5 * * * 0') } with cron
   // triggers { pollSCM ('*/2 * * * 0')} with pollscm
    parameters { 
        string(name: 'DOTNET_BUILD', defaultValue: 'build', description: 'dotnet build') // only one option it is dotnet build
        string(name: 'DOTNET_RESTORE', defaultValue: 'restore', description: 'dotnet restore') // only one option it is dotnet restore 
    }
    //parameters {
        //choice(name: 'DOTNET_RESTORE', choices: [ 'restore', 'test', 'build', 'run test' ], description: 'dotnet restore') // multiple options it is dotnet restore
        //choice(name: 'DOTENET_BUILD', choices: [ 'restore', 'test', 'build', 'run test' ], description: 'dotnet build') // multiple options it is dotnet build
    //}
    stages {
        stage ('vcs') {
            steps {
                git url: 'https://github.com/Madhuri-chinta/MusicStore.git',
                    branch: 'main'
            }
        }
        stage ('Sonarqube Analysis') { 
            steps {
                //sh 'dotnet tool install --global dotnet-sonarscanner'
                sh 'export PATH="$PATH:/root/.dotnet/tools"'
                sh 'ls -al /home/ubuntu/workspace/musicstore'
                sh 'sudo /root/.dotnet/tools/dotnet-sonarscanner begin /k:"Jenkins123" /d:sonar.host.url="http://43.205.129.12:32768"  /d:sonar.token="squ_9d02faff0e73e8a7d6c83f5149e6ee4108da9896"'
                sh 'dotnet build /home/ubuntu/workspace/musicstore/MusicStore/MusicStore.csproj'
                sh 'sudo /root/.dotnet/tools/dotnet-sonarscanner end /d:sonar.token="squ_9d02faff0e73e8a7d6c83f5149e6ee4108da9896"'
             }
        }   
        stage ('build') {
            steps {
                sh "dotnet ${params.DOTNET_RESTORE} ./MusicStore/MusicStore.csproj"
                sh "dotnet ${params.DOTNET_BUILD} ./MusicStore/MusicStore.csproj"
            }
        }
        
        
        stage ('test') {
            steps {
                //sh 'dotnet test ./MusicStoreTest/MusicStoreTest.csproj'
                sh 'dotnet test --logger "junit;LogFilePath=**/surefire-reports/TEST-musicstoretest.xml" ./MusicStoreTest/MusicStoreTest.csproj'
                junit testResults: '**/surefire-reports/*.xml'
            }
        }
        stage ('npm') {
            steps {
                sh 'cd /home/ubuntu/workspace/musicstore/MusicStore/ClientApp'
                sh 'npm install express'
            }
        } 
    }
    post {
        success {
           mail subject : "Jenkins Build of ${JOB_NAME} with id ${BUILD_ID} is success",
                body : "Use this URL ${BUILD_URL} for more info",
                from : 'madhuri123@gmail.com', // here pass jenkins environmental variable 
                to : 'sweety123@gmail.com' // here also pass jenkins environmental variable 
        }
        failure {
            mail subject : "Jenkins Build of ${JOB_NAME} with id ${BUILD_ID} is failure",
                 body : "Use this URL ${BUILD_URL} for more info",
                 from : 'madhuri123@gmail.com',
                 to : "${GIT_AUTHOR_EMAIL}"
        }   
    }
}





