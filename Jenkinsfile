pipeline {
    agent { label 'Linux' }
    tools {
        maven 'maven'
    }

    stages {

        stage('Build Maven') {
            steps{
                 git branch: 'master', credentialsId: 'devopshint', url: 'https://github.com/The-DevOps-2021/Artifactorty_push'
                 sh "mvn -Dmaven.test.failure.ignore=true clean package"
                
            }
        }   
   //stage("Publish to Artifactory") {

            steps {

                script {

                    pom = readMavenPom file: "pom.xml";

                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");

                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"

                    artifactPath = filesByGlob[0].path;

                    artifactExists = fileExists artifactPath;

                    if(artifactExists) {

                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";

                        rtUpload(
                            serverId: 'Artifactory',
                            
                            protocol: 'http',

                            nexusUrl: 'http://3.108.236.103:8082/',

                            groupId: 'pom.com.mycompany.app',

                            version: 'pom.1.0-SNAPSHOT',

                            repository: 'maven-central-repository',

                            credentialsId: 'Artifactory_token',

                            artifacts: [

                                [artifactId: 'pom.my-app',

                                classifier: '',

                                file: artifactPath,

                                type: pom.packaging],

                                [artifactId: 'pom.my-app',

                                classifier: '',

                                file: "pom.xml",

                                type: "pom"]

                            ]

                        );

                    } else {

                        //error "*** File: ${artifactPath}, could not be found";

                    }

                }

            }

        }
    }
}
