# JENKINSFILE: 

1. **Dependency Check Step**:
   - The `dependencyCheck` step in your script is used to execute the Dependency Check tool itself, but it seems there might be confusion with the syntax or usage of parameters.

2. **Trivy Step**:
   - The `sh` step is used to execute shell commands. However, the `trivy fs .` command might not be valid depending on how Trivy is installed and configured on your Jenkins environment.



```groovy
pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git 'https://github.com/KGoksal/CountryBank-JenkinsFile.git'
            }
        }

        stage('OWASP Dependency Check') {
            steps {
                script {
                    // Run Dependency Check
                    def dependencyCheck = tool name: 'DC', type: 'org.jenkinsci.plugins.DependencyCheck.DependencyCheckInstaller'
                    sh "${dependencyCheck}/bin/dependency-check.sh --scan . --format XML --out ."
                }
            }
        }

        stage('Trivy') {
            steps {
                script {
                    // Run Trivy
                    sh "trivy --quiet fs ."
                }
            }
        }

        stage('Build & deploy') {
            steps {
                script {
                    // Example command to build and deploy using Docker Compose
                    sh "docker-compose up -d"
                }
            }
        }
    }

    post {
        always {
            // Archive Dependency Check XML report
            archiveArtifacts '**/dependency-check-report.xml'
        }
    }
}
```

### Explanation:

- **Git Checkout Stage**: Checks out the repository from GitHub.
  
- **OWASP Dependency Check Stage**:
  - Uses the `tool` step to ensure that the Dependency Check tool (`DC`) is installed and configured. Adjust `type` parameter (`org.jenkinsci.plugins.DependencyCheck.DependencyCheckInstaller`) according to your Jenkins configuration.
  - Executes Dependency Check using the `sh` step. It runs the `dependency-check.sh` script with the `--scan .` option to scan all files in the workspace directory (`.`) for vulnerabilities. Adjust `--format` and `--out` options as needed.

- **Trivy Stage**:
  - Executes Trivy with the `sh` step. Adjust the command based on how Trivy is installed on your Jenkins environment. Here, `trivy --quiet fs .` is used to scan the filesystem (`fs`) starting from the current directory (`.`). Adjust options (`--quiet`, `--exit-code`) as per your Trivy configuration.

- **Build & Deploy Stage**: Example step to build and deploy using Docker Compose. Replace with your actual build and deployment commands.

- **Post Section**: Archives the Dependency Check XML report (`dependency-check-report.xml`) as artifacts. Adjust the pattern (`**/dependency-check-report.xml`) according to your report file name and location.

### Notes:
- Make sure that Dependency Check (`DC`) and Trivy are correctly installed and configured on your Jenkins server.
- Adjust paths and options (`--scan`, `--format`, `--out`, etc.) in Dependency Check and Trivy commands based on your project setup and tool versions.
- Always test changes in a development environment before applying them to production.


************************************************************


[![Build Status](https://travis-ci.org/nikitap492/CountryBank.svg?branch=master)](https://travis-ci.org/nikitap492/CountryBank)
[![codecov.io](https://codecov.io/github/nikitap492/CountryBank/coverage.svg?branch=master)](https://travis-ci.org/nikitap492/CountryBank?branch=master)
[![CodeFactor](https://www.codefactor.io/repository/github/nikitap492/countrybank/badge)](https://www.codefactor.io/repository/github/nikitap492/countrybank)
# Country Bank
**Simple bank application with Spring Boot and Spring Data**

### Country bank is standalone server by spring boot with flexible interface.
Imagine yourself town that exists on the Wild West and has population about 1000 people. Country Bank is located in the town.  Simple application was developed for bank which offers e-services (Are you sure there is on the Wild West?) 

### Clients might:
- Subscribe. Anonymous user might subscribe by email. Authenticated user just needs click a button for subscribe/ unsubscribe (email will taken from account)
- Create account. After sign up in system, registration token will have sent for confirmatioin of registration
- Send message to the bank. Messages are saving in database. Authenticated user just  needs wrote a message text  (email and name will taken from account)
- Reset password if user forget it.  Reset password token will have sent on email

All tokens valid 24 hours.


![anon2](https://cloud.githubusercontent.com/assets/18111582/22162520/13a3254c-df60-11e6-936a-4ac52a765e7f.gif)


After authentication you can use other features, as 
- Transfer money to bill
- Make payments for fines, debts, or public services (For simplicity this all was combines in one action – make payment. Then let the government chooses  for which payment has been   )
- Take credits or put their money in deposit 
- Check bills or open new bill.
- Check transactions by bill
- Choose active bill

#### For simplicity also all bills have UUID structure in spite of  XXXX-XXXX-XXXX-XXXX. Application is used to active bill for payments.

![auth3](https://cloud.githubusercontent.com/assets/18111582/22162518/13a12e22-df60-11e6-8852-b1199ce0dd3e.gif)

### Application structure is using MVC pattern:
![layers](https://cloud.githubusercontent.com/assets/18111582/22162519/13a2dc90-df60-11e6-8bc3-b5effc15f4c0.gif)
Controllers are receiving all requests, after checking by validators. If request are valid then services process this interaction with database.
## How To Install?
### By gradle
- You must have jdk 8 and gradle
- Download sources from github
- You need export environment variables for mail provider: `CONFIG_MAIL_HOST`, `CONFIG_MAIL_PORT`, `CONFIG_MAIL_SMTP_AUTH`, `CONFIG_MAIL_STARTTLS`, `CONFIG_MAIL_USERNAME`, `CONFIG_MAIL_PASSWORD` or override these variable in `Application.yml` in classpath.  
- Run `gradle bootRun`, then  server starts on port `8000`

### By docker
- Install docker and docker-compose
- You need export environment variables for mail provider: `CONFIG_MAIL_HOST`, `CONFIG_MAIL_PORT`, `CONFIG_MAIL_SMTP_AUTH`, `CONFIG_MAIL_STARTTLS`, `CONFIG_MAIL_USERNAME`, `CONFIG_MAIL_PASSWORD`.
- In your terminal run `docker-compose up`, then  server starts on port `8000`

