pipeline {
    agent any
         tools {
            image 'ruby:3.2'
            args '-u root'
        }
    // tools {
    //     jdk 'jdk21'
    // }

    // environment {
    //     SONARQUBE_ENV = 'SonarQube'
    //     DOCKER_IMAGE = "yourdockerhub/rails-app"
    //     AWS_DEFAULT_REGION = 'us-west-1'
    //     // RECIPIENTS = 'yourmail@gmail.com'
    // }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/satyanarayana-24/rails_added_adminpanel_and_action_mailer_to_fb_app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                gem install bundler
                bundle install
                '''
            }
        }

        stage('Setup Database') {
            steps {
                sh '''
                cp config/database.yml.example config/database.yml || true
                bundle exec rails db:create
                bundle exec rails db:migrate
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh 'bundle exec rails test || true'
            }
        }

        stage('Precompile Assets') {
            steps {
                sh 'bundle exec rake assets:precompile'
            }
        }

        stage('Package Artifact') {
            steps {
                sh 'zip -r rails-build.zip .'
            }
        }

        // stage('SonarQube Analysis') {
        //     steps {
        //         script {
        //             def scannerHome = tool 'SonarScanner'
        //             withSonarQubeEnv('SonarQube') {
        //                 sh """
        //                 ${scannerHome}/bin/sonar-scanner \
        //                 -Dsonar.projectKey=rails-app \
        //                 -Dsonar.sources=.
        //                 """
        //             }
        //         }
        //     }
        // }

        // stage('Quality Gate') {
        //     steps {
        //         timeout(time: 2, unit: 'MINUTES') {
        //             waitForQualityGate abortPipeline: true
        //         }
        //     }
        // }

        // stage('Upload to Nexus') {
        //     steps {
        //         withCredentials([usernamePassword(
        //             credentialsId: 'nexus-cred',
        //             usernameVariable: 'NEXUS_USER',
        //             passwordVariable: 'NEXUS_PASS'
        //         )]) {
        //             sh '''
        //             curl -u $NEXUS_USER:$NEXUS_PASS \
        //             --upload-file rails-build.zip \
        //             http://localhost:8081/repository/raw-hosted/rails-build-${BUILD_NUMBER}.zip
        //             '''
        //         }
        //     }
        // }

        // stage('Build Docker Image') {
        //     steps {
        //         sh '''
        //         docker build -t $DOCKER_IMAGE:${BUILD_NUMBER} .
        //         docker tag $DOCKER_IMAGE:${BUILD_NUMBER} $DOCKER_IMAGE:latest
        //         '''
        //     }
        // }

        // stage('Push Docker Image') {
        //     steps {
        //         withCredentials([usernamePassword(
        //             credentialsId: 'dockerhub-cred',
        //             usernameVariable: 'USER',
        //             passwordVariable: 'PASS'
        //         )]) {
        //             sh '''
        //             echo $PASS | docker login -u $USER --password-stdin
        //             docker push $DOCKER_IMAGE:${BUILD_NUMBER}
        //             docker push $DOCKER_IMAGE:latest
        //             '''
        //         }
        //     }
        // }

        // stage('Deploy to EKS') {
        //     steps {
        //         sh '''
        //         aws eks update-kubeconfig --region us-west-1 --name myclusterr
        //         kubectl apply -f deployment.yml
        //         kubectl apply -f service.yml
        //         '''
        //     }
        // }
    }
}
