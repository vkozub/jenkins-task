pipeline {
    agent any
    
    stages {
        stage('Deploy Apache2') {
            steps {
                script {
                    echo "Starting Apache2 installation on Azure VM..."
                    
                    sh '''
                        #!/bin/bash
                        set -e
                        
                        # Update system
                        sudo apt-get update
                        
                        # Install Apache2
                        sudo apt-get install -y apache2
                        
                        # Start Apache2
                        sudo systemctl start apache2
                        sudo systemctl enable apache2
                        
                        # Verify installation
                        echo "Apache2 installed successfully"
                        apache2 -v
                        
                        # Check if running
                        sudo systemctl status apache2
                    '''
                }
            }
        }
    }
    
    post {
        success {
            echo "Apache2 deployment successful!"
        }
        failure {
            echo "Apache2 deployment failed!"
        }
    }
}
