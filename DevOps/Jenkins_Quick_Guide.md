# ⚙️ Jenkins Quick Guide

Jenkins is an open-source automation server widely used for **CI/CD**. It allows developers to build, test, and deploy applications automatically.

---

## 🔧 Key Concepts

| Concept         | Description                                                                 |
|----------------|-----------------------------------------------------------------------------|
| **Job**         | A task or set of tasks (like build, test, deploy) defined in Jenkins.       |
| **Pipeline**    | A set of steps (stages) written in Groovy DSL to define the workflow.       |
| **Agent**       | A machine (or container) where Jenkins executes jobs.                       |
| **Executor**    | A slot for running one job at a time on an agent.                           |
| **Node**        | Any machine connected to Jenkins (master or worker).                        |
| **Workspace**   | Directory on the node where Jenkins stores files for the job.               |
| **Artifact**    | Files created during a build (e.g., binaries, logs) that are saved/exported.|

---

## 🚀 Freestyle vs Pipeline

- **Freestyle Job**: Simple GUI-based configuration.
- **Pipeline Job**: Code-as-configuration using `Jenkinsfile` with full control and versioning.

---

## 📝 Jenkinsfile Example (Declarative)

```
pipeline {
  agent any

  environment {
    APP_ENV = 'development'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        echo 'Building the project...'
      }
    }

    stage('Test') {
      steps {
        echo 'Running tests...'
      }
    }

    stage('Deploy') {
      when {
        branch 'main'
      }
      steps {
        echo 'Deploying to production...'
      }
    }
  }

  post {
    success {
      echo 'Pipeline completed successfully.'
    }
    failure {
      echo 'Pipeline failed.'
    }
  }
}
```

---

## 🔄 Trigger Options

| Trigger                     | Description                                            |
|----------------------------|--------------------------------------------------------|
| `pollSCM`                  | Periodically check source control for changes.         |
| `cron`                     | Run on a schedule.                                     |
| GitHub Webhook             | Trigger on push or pull request.                       |
| Manual Trigger             | Start pipeline by clicking **Build Now**.              |

---

## 🧰 Plugins to Know

- **Git Plugin**: Integrates Git SCM.
- **Pipeline Plugin**: Required for Jenkinsfile support.
- **Blue Ocean**: Visual interface for pipelines.
- **Credentials Binding**: For securely managing secrets.
- **Docker Pipeline**: If you’re building inside containers.
- **Slack Notification**: For alerts in your team channel.

---

## ✅ Best Practices

- Use `Jenkinsfile` in version control.
- Run builds in isolated environments (e.g., Docker).
- Archive build artifacts and test results.
- Use stages for clear separation of concerns.
- Store secrets in Jenkins credentials store.
- Enable build status notifications (Slack, Email).

---

# 📎 Jenkins Appendix: Docker, Kubernetes, Reusable Libraries, Terraform & Helm Pipelines

---

## 🐳 Jenkins + Docker

Build and run your app inside Docker containers to ensure environment consistency.

```
pipeline {
  agent {
    docker {
      image 'node:18-alpine'
      args '-u root:root'
    }
  }

  stages {
    stage('Install') {
      steps {
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        sh 'npm test'
      }
    }

    stage('Build') {
      steps {
        sh 'npm run build'
      }
    }
  }
}
```

🧠 **Note:** Requires Docker installed and accessible to Jenkins agent.

---

## ☸️ Jenkins + Kubernetes (Using Kubernetes Plugin)

Run each stage in a containerized pod using Kubernetes.

```
pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: maven
    image: maven:3.8.5-openjdk-17
    command:
    - cat
    tty: true
  - name: node
    image: node:18-alpine
    command:
    - cat
    tty: true
"""
    }
  }

  stages {
    stage('Build') {
      steps {
        container('maven') {
          sh 'mvn clean package'
        }
      }
    }

    stage('Test') {
      steps {
        container('node') {
          sh 'npm install && npm test'
        }
      }
    }
  }
}
```

🔒 **Tip:** Use Kubernetes service accounts and namespaces for isolation.

---

## 🔁 Jenkins Shared Library (Reusable Pipelines)

Extract common pipeline logic into shared Groovy libraries stored in a Git repo.

### 1. Structure of the shared library (in Git):
```
(root)
└── vars/
    └── deployApp.groovy
```

### 2. Content of `deployApp.groovy`:
```
def call(String serviceName) {
  pipeline {
    agent any

    stages {
      stage("Build ${serviceName}") {
        steps {
          echo "Building ${serviceName}..."
        }
      }
      stage("Deploy ${serviceName}") {
        steps {
          echo "Deploying ${serviceName} to production..."
        }
      }
    }
  }
}
```

### 3. Jenkinsfile using shared library:
```
@Library('my-shared-library') _

deployApp('orders-service')
```

🧠 **Note:** Configure the shared library repo in **Manage Jenkins → Configure System → Global Pipeline Libraries**

---

## 🌍 Jenkins + Terraform

Use Jenkins to plan and apply infrastructure changes with Terraform.

```
pipeline {
  agent any

  environment {
    TF_WORKSPACE = 'prod'
  }

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/your-org/terraform-infra-repo.git'
      }
    }

    stage('Terraform Init') {
      steps {
        sh 'terraform init'
      }
    }

    stage('Terraform Plan') {
      steps {
        sh "terraform workspace select ${TF_WORKSPACE} || terraform workspace new ${TF_WORKSPACE}"
        sh 'terraform plan -out=tfplan'
      }
    }

    stage('Terraform Apply') {
      when {
        branch 'main'
      }
      steps {
        input message: 'Approve Terraform apply?'
        sh 'terraform apply tfplan'
      }
    }
  }
}
```

🔐 **Tip:** Store Terraform credentials (e.g., AWS, GCP) in Jenkins credentials and inject them into the environment securely.

---

## ⛵ Jenkins + Helm (for Kubernetes Deployments)

Use Helm in a Jenkins pipeline to deploy applications to Kubernetes clusters.

```
pipeline {
  agent any

  environment {
    KUBECONFIG = credentials('kubeconfig-jenkins')
  }

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/your-org/helm-charts.git'
      }
    }

    stage('Helm Lint') {
      steps {
        dir('my-chart') {
          sh 'helm lint .'
        }
      }
    }

    stage('Helm Dry Run') {
      steps {
        dir('my-chart') {
          sh 'helm upgrade --install my-app . --namespace prod --dry-run'
        }
      }
    }

    stage('Helm Deploy') {
      when {
        branch 'main'
      }
      steps {
        dir('my-chart') {
          sh 'helm upgrade --install my-app . --namespace prod'
        }
      }
    }
  }
}
```

📦 **Tip:** Helm dependencies? Run `helm dependency update` before deploying.

---
