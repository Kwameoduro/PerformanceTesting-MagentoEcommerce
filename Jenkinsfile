// The Jenkins Pipeline for Magento

pipeline {
	agent {
		label 'Jenkins-agent'
    }

    stages {
		stage('Checkout') {
			steps {
				git branch: 'main', url: 'https://github.com/Kwameoduro/PerformanceTesting-MagentoEcommerce.git'
            }
        }

        stage('Run JMeter Test') {
			steps {
				sh 'rm -rf reports results.jtl || true'

             sh '''
                jmeter -n -t load-test.jmx \
                       -l results.jtl \
                       -e -o reports
                '''
            }
        }

        stage('Publish Report') {
			steps {
				publishHTML(target: [
                    allowMissing: false,
                    keepAll: true,
                    reportDir: 'reports',
                    reportFiles: 'index.html',
                    reportName: 'JMeter HTML Report'
                ])
            }
        }
    }

    post {
		always {
			archiveArtifacts artifacts: 'results.jtl, reports/**', fingerprint: true
        }
    }
}