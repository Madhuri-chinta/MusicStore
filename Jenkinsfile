pipeline {
    agent { label 'UBUNTU_NODE1'}
    stages {
        stage ('vcs') {
            steps {
                git url: 'https://github.com/Madhuri-chinta/MusicStore.git',
                    branch: 'main'
            }
        
        }
        stage ('build') {
            steps {
                sh 'dotnet restore ./MusicStore/MusicStore.csproj'
                sh 'dotnet build ./MusicStore/MusicStore.csproj'
            }
        }
        stage ('npm') {
            steps {
                sh 'cd /home/ubuntu/workspace/music-scr/MusicStore/ClientApp'
                sh 'npm install express'
            }
        } 
           }
}