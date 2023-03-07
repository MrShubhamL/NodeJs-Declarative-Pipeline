# NodeJs-Declarative-Pipeline
Declarative Pipeline Project for Node JS

    pipeline{
        agent any
        stages{
            stage('Build'){
                steps{
                    script{
                        git 'https://github.com/fsojitra/Node.js-Register-Login-App.git'
                        nodejs('nodejs') {
                           sh 'npm install'
                        }
                    }
                }
            }
            stage('Test'){
                steps{
                    script{
                        git 'https://github.com/fsojitra/Node.js-Register-Login-App.git'
                        nodejs('nodejs') {
                           sh 'npm install'
                        }
                        sh 'tar -czf nodejsproject.tar.gz *'
                    }
                }
            }
            stage('Deploy'){
                steps{
                    script{
                        git 'https://github.com/fsojitra/Node.js-Register-Login-App.git'
                        nodejs('nodejs') {
                           archiveArtifacts artifacts: 'nodeproject.tar.gz', followSymlinks: false
                           sshPublisher(publishers: [sshPublisherDesc(configName: 'NodeJS', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'tar -xzf nodeproject.tar.gz && sudo systemctl restart nodejsproject.service', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'nodeproject*.tar.gz')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
                        }
                    }
                }
            }
        }
    }
