pipeline {
    agent { label 'UBUNTU_NODE1'}
    //triggers { cron ('*/5 * * * 0') } with cron
   // triggers { pollSCM ('*/2 * * * 0')} with pollscm
    parameters { 
        string(name: 'DOTNET_BUILD', defaultValue: 'build', description: 'dotnet build') // only one option
        choice(name: 'DOTNET_RESTORE', choices: [ 'restore', 'test', 'build', 'run test' ], description: 'dotnet restore') // multiple options
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
                sh 'dotnet test ./MusicStoreTest/MusicStoreTest.csproj'
            }
        }
        stage ('npm') {
            steps {
                sh 'cd /home/ubuntu/workspace/musicstore/MusicStore/ClientApp'
                sh 'npm install express'
            }
        } 
       }
}
