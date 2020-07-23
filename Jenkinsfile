pipeline {

    agent any
    
    parameters {

        choice (
            name: "START", choices: ["NO", "YES"], description: "To start or not to start"
        )
    }

    stages {

        stage("SETUP") {

            when {
                expression {
                    !fileExists(".env") && !fileExists("config/app.config.json") && !fileExists("config/key.store.json")
                }
            }

            steps {

                withCredentials ([

                    file(credentialsId: "env", variable: "environment"),
                    file(credentialsId: "config", variable: "configuration"),
                    file(credentialsId: "key", variable: "keystore"),
                ]) {
                    sh "cp \$environment .env"
                    sh "cp \$configuration config/app.config.json"
                    sh "cp \$keystore config/key.store.json"
                }
            }
        }

        stage("BUILD") {

            steps {

                sh "npm i --save"
            }
        }

        stage("COMPILE") {

            steps {

                sh "truffle compile"            
            }
        }

        stage("MIGRATE") {

            when {
                expression {
                    params.START == "YES"
                }
            }

            steps {

                sh "truffle migrate --network ropsten"
            }
        }
    }
}