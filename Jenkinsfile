pipeline {
    agent { label 'UBUNTU_NODE1'}
    //triggers { cron ('*/5 * * * 0') } with cron
   // triggers { pollSCM ('*/2 * * * 0')} with pollscm
    parameters { 
        string(name: 'DOTNET_BUILD', defaultValue: 'build', description: 'dotnet build') // only one option it is dotnet build
        string(name: 'DOTNET_RESTORE', defaultValue: 'restore', description: 'dotnet restore') // only one option it is dotnet restore 
    }
    parameters {
        choice(name: 'DOTNET_RESTORE', choices: [ 'restore', 'test', 'build', 'run test' ], description: 'dotnet restore') // multiple options it is dotnet restore
        choice(name: 'DOTENET_BUILD', choices: [ 'restore', 'test', 'build', 'run test' ], description: 'dotnet build') // multiple options it is dotnet build
    }
    stages {
        stage ('vcs') {
            steps {
                git url: 'https://github.com/Madhuri-chinta/MusicStore.git',
                    branch: 'main'
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
        failure {
           mail subject : "Jenkins Build of ${JOB_NAME} with id ${BUILD_ID} is failure",
                body : "Use this URL ${BUILD_URL} for more info",
                from : "${GIT_COMMITTER_EMAIL}", // here pass jenkins environmental variable 
                to : "${GIT_AUTHOR_EMAIL}" // here also pass jenkins environmental variable 
        }
        success {
            mail subject : "Jenkins Build of ${JOB_NAME} with id ${BUILD_ID} is success",
                 body : "Use this URL ${BUILD_URL} for more info",
                 from : 'madhu123@gmail.com',
                 to : 'sweety123@gmail.com'
        }   
}
}
