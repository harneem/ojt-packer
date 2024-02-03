pipeline {
    agent any

    environment {
        AWS_CREDENTIALS = credentials('packer-install')
    }

    stages {

        stage('Checkout Source') {
            steps {
                git branch: "master",
                url: 'https://github.com/harneem/ojt-packer.git'
            }
        }

        stage('packer plugin') {
            steps {
                script {
                    sh 'packer plugins install github.com/hashicorp/amazon'
                }
            }
        }

        stage('packer validate') {
            steps {
                script {
                    sh 'packer validate -var-file=vars.json template.json'
                }
            }
        }

        stage('packer build') {
    steps {
        script {
            sh 'packer build -var-file=vars.json template.json'
            
            // Read the manifest.json file and extract the AMI ID
            def manifest = readJSON file: 'manifest.json'
            def latestAmiId = manifest.builds[0].artifact_id.split(':').last().replaceAll('"', '').trim()

            // Store the AMI ID as an environment variable
            env.LATEST_AMI_ID = latestAmiId

            // Print the latest AMI ID for logging purposes
            echo "Latest AMI ID: $latestAmiId"
        }
    }
}


        // Add more stages as needed
    }
}
          
