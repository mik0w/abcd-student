pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage('Code checkout from GitHub') {
            steps {
                script {
                    cleanWs()
                    git credentialsId: 'github_pat', url: 'https://github.com/mik0w/abcd-student', branch: 'main'
                }
            }
        }
        stage('Example') {
            steps {
                echo 'Hello!'
                sh 'ls -la'
            }
        }
        // stage('[ZAP] Baseline passive-scan') {
        //     steps {
        //         sh 'mkdir -p results/'
        //         sh '''
        //             docker run --name juice-shop -d --rm \
        //                 -p 3000:3000 \
        //                 bkimminich/juice-shop
        //             sleep 5
        //         '''
        //         sh '''
        //             docker run --name zap \
        //                 --user=root --add-host=host.docker.internal:host-gateway \
        //                 -v /home/mik0w_test/abcd/abcd-student/.zap:/zap/wrk/:rw \
        //                 -t ghcr.io/zaproxy/zaproxy:stable bash -c \
        //                 "mkdir -p /zap/wrk/reports && zap.sh -cmd -addonupdate; zap.sh -cmd -addoninstall communityScripts -addoninstall pscanrulesAlpha -addoninstall pscanrulesBeta -autorun /zap/wrk/passive.yaml" \
        //                 || true
        //         '''
        //         // sh '''
        //         //     docker run  --user=root -v /home/mik0w_test/abcd/abcd-student/.zap:/zap/wrk/:rw -t ghcr.io/zaproxy/zaproxy:stable touch /zap/wrk/test.txt
        //         // '''
        //     }
        //     post {
        //         always {
        //             sh '''
        //                 docker cp zap:/zap/wrk/reports/zap_html_report.html ${WORKSPACE}/results/zap_html_report.html
        //                 docker cp zap:/zap/wrk/reports/zap_xml_report.xml ${WORKSPACE}/results/zap_xml_report.xml
        //                 docker stop zap juice-shop
        //                 docker rm zap
        //             '''
                    
        //             defectDojoPublisher(
        //                 artifact: "/home/mik0w_test/abcd/abcd-student/.zap/results/zap_xml_report.xml", 
        //                 productName: 'Juice Shop', 
        //                 scanType: 'ZAP Scan', 
        //                 engagementName: 'mikolaj@ardoq.com'
        //             )
        //         }
        //     }

        // }
        // stage('[OSV] Vulnerability Scan') {
        //     steps {
        //         script {
        //             echo 'Running OSV-Scanner on JuiceShop...'
        //             sh '''
        //                 mkdir -p results/
        //                 ls -la
        //                 /go/bin/osv-scanner --lockfile=package-lock.json --format json --output=results/result.json
                        
        //             '''
        //         }
        //     }
        //     post {
        //         always {
        //             sh '''
        //                 cd results/
        //                 pwd
        //                 cat result.json
        //             '''
        //             defectDojoPublisher(
        //                  artifact: "/var/jenkins_home/workspace/ABCD_pipeline/results/result.json", 
        //                  productName: 'Juice Shop', 
        //                  scanType: 'OSV Scan', 
        //                  engagementName: 'mikolaj@ardoq.com'
        //              )
        //         }
        //     }
        // }
        stage('Trufflehog Scan') {
            steps {
                script {
                    echo 'Running TruffleHog scan on JuiceShop repo...'
                    // sh '''
                    //     mkdir -p results/
                    //     ls -la
                    //     /go/bin/osv-scanner --lockfile=package-lock.json --format json --output=results/result.json
                        
                    // '''
                    sh '''
                        trufflehog git file://. --only-verified
                    '''
                }
            }
            // post {
            //     always {
            //         sh '''
            //             cd results/
            //             pwd
            //             cat result.json
            //         '''
            //         defectDojoPublisher(
            //              artifact: "/var/jenkins_home/workspace/ABCD_pipeline/results/result.json", 
            //              productName: 'Juice Shop', 
            //              scanType: 'OSV Scan', 
            //              engagementName: 'mikolaj@ardoq.com'
            //          )
            //     }
            // }
        }
    }
}
