pipeline {
    agent { label 'UBUNTU_NODE1'}
    //triggers { cron ('*/5 * * * 0') }
    parameters { 
        string(name: 'DOTNET_BUILD', defaultValue: 'build', description: 'dotnet build') 
        choice(name: 'DOTNET_RESTORE', choices: [ 'restore', 'test', 'build', 'run test' ]description: 'dotnet restore')
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
        stage ('npm') {
            steps {
                sh 'cd /home/ubuntu/workspace/musicstore/MusicStore/ClientApp'
                sh 'npm install express'
            }
        } 
           }
}
