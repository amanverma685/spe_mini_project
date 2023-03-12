pipeline{

    environment{
	    registry="docker685/docker_repo_spe"
	    registryCredential="[;,$j8q_xAR6YVw"
	    dockerImage=""
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')

	}

	agent any

    stages{
        stage('Git clone'){
            steps{
                git branch: 'master', url: 'https://github.com/amanverma685/spe_mini_project.git'
            }
        }
    	stage('Maven Build'){
        	steps{
            	echo 'This is job building stage'
            	//maven clean and install command to build the target folder
            	sh "mvn clean install"
            }
        }
        stage('Build Docker Image') {
              steps {
                sh 'docker build -t docker685/jenkins-docker-hub .'
              }
            }
            stage('Login into DockerHub') {
              steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
              }
            }
            stage('Push to DockerHub') {
              steps {
                sh 'docker push docker685/jenkins-docker-hub'
              }
            }

//         stage("Building Docker Image"){
//             steps{
//                 script{
//                     // dockerImage = docker.build registry + ":latest"
//                     dockerImage = docker.build("docker685/spe_mini_proj_scientific_calc:latest", "./")
//                 }
//             }
//         }
//         stage("Deploy out Image to DockerHub"){
//             steps{
//                 script{
//                     withCredentials([string(credentialsId: 'dockerhub_pwd', variable: 'dockerhub_pwd')]) {
//                         // some block
//                         sh "docker login -u docker685 -p [;,$j8q_xAR6YVw"
//                         sh "docker push docker685/spe_mini_proj_scientific_calc:latest"
//                     }
//                 }
//             }
//         }
//
//         stage("Delete Docker Image from local system"){
//             steps{
//                 sh "docker rmi docker685/spe_mini_proj_scientific_calc:latest"
//             }
//         }

        stage("Ansible Deploy"){
            steps{
                ansiblePlaybook colorized: true, disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory', playbook: 'plybk.yml'
            }
        }
    }
}
