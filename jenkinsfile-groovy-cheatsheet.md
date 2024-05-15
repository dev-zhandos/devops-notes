# Jenkinsfile (Pipeline as Code) and Groovy Cheat Sheet

Jenkins pipelines, defined in `Jenkinsfile` using the Groovy DSL, automate your CI/CD workflows. This cheat sheet covers essential syntax, common patterns, and Groovy features useful in Jenkins pipelines.


## Table of Contents
1. Pipeline Structure
2. Directives
3. Stages and Steps
4. Groovy Fundamentals
5. Common Pipeline Patterns
6. Best Practices
7. Troubleshooting

## 1. Pipeline Structure

```groovy
pipeline {
    agent { label 'linux' }  // Where to run (node label, docker, any)
    options { ... }          // Pipeline-level options (e.g., timeout, retry)
    parameters { ... }        // Input parameters for the pipeline
    environment { ... }      // Define environment variables
    triggers { ... }         // Automate pipeline execution (e.g., cron, pollSCM)
    stages { ... }           // Define the pipeline workflow (required)
    post { ... }             // Actions to run after each stage or the pipeline
}
```


## 2. Directives

- `agent`: Defines where the pipeline (or a specific stage) should execute.
    - `any`: Runs on any available agent.
    - `label 'labelName'`: Runs on an agent with the given label.
    - `docker`: Runs inside a Docker container.
- `options`: Configures pipeline-level options.
    - `timeout(time: 1, unit: 'HOURS')`: Sets a timeout for the pipeline.
    - `retry(3)`: Retries the pipeline up to 3 times on failure.
    - `disableConcurrentBuilds()`: Prevents concurrent builds.
- `parameters`: Defines input parameters for the pipeline.
    - `string(name: 'BRANCH', defaultValue: 'main', description: 'Branch to build')`
- `environment`: Sets environment variables.
    - `APP_VERSION = '1.0.0'`
- `triggers`: Defines automatic triggers for the pipeline.
    - `cron('H/15 * * * *')`: Runs every 15 minutes.
    - `pollSCM('H/5 * * * *')`: Polls the SCM for changes every 5 minutes.
- `post`: Defines actions to run after a stage or the entire pipeline.
    - `always { ... }`: Runs always (success, failure, unstable).
    - `success { ... }`: Runs only on success.
    - `failure { ... }`: Runs only on failure.


## 3. Stages and Steps

```groovy
stages {
    stage('Build') {
        steps {
            sh 'mvn clean package'  // Shell command
            // Other steps: bat (Windows), powershell, etc.
        }
    }
    stage('Test') { ... }
    stage('Deploy') { ... }
}
```
- Each `stage` represents a logical section of your pipeline.
- Inside `steps`, you define the actual actions to perform.


## 4. Groovy Fundamentals

- **Variables:**  Dynamically typed.
    - `def name = 'Alice'`
- **Strings:**  Single (`'`) or double (`"`) quotes.
    - `String message = "Hello, ${name}!"` (string interpolation)
- **Collections:**
    - `List numbers = [1, 2, 3]`
    - `Map person = [name: 'Bob', age: 30]`
- **Conditional Logic:** `if`, `else if`, `else`.
- **Loops:** `for`, `while`.
- **Functions (closures):**
    - `def greet(name) { echo "Hello, ${name}!" }`
- **Error Handling:** `try`, `catch`, `finally`.


## 5. Common Pipeline Patterns

- **Parallel Stages:**
```groovy
parallel {
    stage('Build') { ... }
    stage('Test') { ... }
}
```
- **Conditional Steps:**
```groovy
stage('Deploy') {
    when {
        branch 'main' 
    }
    steps { ... } // Only run if the branch is 'main'
}
```


## 6. Best Practices
- Use declarative pipelines whenever possible.
- Keep the Jenkinsfile in version control alongside your code.
- Use descriptive stage names and step descriptions.
- Modularize your pipeline with shared libraries.
- Securely handle credentials.


## 7. Troubleshooting
- View pipeline logs in the Jenkins UI.
- Use the `echo` step to print debug information.
- Run pipelines with the `Replay` option for interactive debugging.