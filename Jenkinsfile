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

        stage('Analyze Apache2 Logs') {
            steps {
                script {
                    echo "Analyzing Apache2 logs for 4xx and 5xx errors..."
                    
                    sh '''
                        #!/bin/bash
                        
                        APACHE_LOG="/var/log/apache2/access.log"
                        
                        echo "=========================================="
                        echo "4xx Client Errors"
                        echo "=========================================="
                        
                        # Count 4xx errors
                        COUNT_4xx=$(sudo grep -oP ' 4[0-9]{2} ' "$APACHE_LOG" | wc -l)
                        echo "Total 4xx errors: $COUNT_4xx"
                        
                        if [ "$COUNT_4xx" -gt 0 ]; then
                            echo ""
                            echo "Breakdown by status code:"
                            sudo grep -oP ' 4[0-9]{2} ' "$APACHE_LOG" | sort | uniq -c | sort -rn
                            echo ""
                            echo "Sample 4xx errors:"
                            sudo grep ' 4[0-9]{2} ' "$APACHE_LOG" | head -5
                        else
                            echo "No 4xx errors found"
                        fi
                        
                        echo ""
                        echo "=========================================="
                        echo "5xx Server Errors"
                        echo "=========================================="
                        
                        # Count 5xx errors
                        COUNT_5xx=$(sudo grep -oP ' 5[0-9]{2} ' "$APACHE_LOG" | wc -l)
                        echo "Total 5xx errors: $COUNT_5xx"
                        
                        if [ "$COUNT_5xx" -gt 0 ]; then
                            echo ""
                            echo "Breakdown by status code:"
                            sudo grep -oP ' 5[0-9]{2} ' "$APACHE_LOG" | sort | uniq -c | sort -rn
                            echo ""
                            echo "Sample 5xx errors:"
                            sudo grep ' 5[0-9]{2} ' "$APACHE_LOG" | head -5
                        else
                            echo "No 5xx errors found"
                        fi
                        
                        echo "=========================================="
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
