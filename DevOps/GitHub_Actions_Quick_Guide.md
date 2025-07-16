# 🚀 GitHub Actions Quick Guide

GitHub Actions is a CI/CD tool built directly into GitHub that allows you to automate tasks like testing, building, and deploying your code when events occur in your repositories.

---

## 🧱 Core Concepts

| Term         | Description |
|--------------|-------------|
| **Workflow** | A configurable automated process, defined in `.yml` files under `.github/workflows/`. |
| **Job**      | A set of steps that run on the same runner. Jobs can run sequentially or in parallel. |
| **Step**     | A single task that can run commands or use an existing action. |
| **Runner**   | A server that runs the workflows. GitHub provides hosted runners (Ubuntu, macOS, Windows). |

---

## ✅ Basic Workflow Example

This example runs tests on every push to `main`.

```
name: Run Tests

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

---

## 🔄 Trigger Types

| Trigger      | Example                             |
|--------------|-------------------------------------|
| `push`       | On push to branch                   |
| `pull_request` | On pull request events          |
| `schedule`   | Cron syntax for periodic runs       |
| `workflow_dispatch` | Manual trigger via GitHub UI |
| `release`    | On creating or publishing releases  |

---

## 🧩 Common Built-in Actions

| Action | Purpose |
|--------|---------|
| `actions/checkout` | Check out repository code |
| `actions/setup-node` | Setup Node.js |
| `actions/setup-python` | Setup Python |
| `actions/upload-artifact` | Save build/test output |
| `actions/cache` | Cache dependencies |

---

## 🚀 Deployment Example (to Vercel, Netlify, Firebase, etc.)

Example of deploying to Firebase:

```
name: Deploy to Firebase

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - run: npm install && npm run build

      - uses: w9jds/firebase-action@v13.0.1
        with:
          args: deploy --only hosting
        env:
          FIREBASE_TOKEN: ${{ secrets.FIREBASE_TOKEN }}
```

---

## 🛠️ Matrix Builds (Multiple Node Versions)

```
name: Test on multiple Node.js versions

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [16, 18, 20]
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm install
      - run: npm test
```

---

## 🔐 Secrets & Environment Variables

Define secrets in **Repo > Settings > Secrets and Variables > Actions**.

Access them in workflows:

```
env:
  API_KEY: ${{ secrets.MY_API_KEY }}
```

---

## 📦 Cache Dependencies

Speed up workflows by caching dependencies:

```
- uses: actions/cache@v3
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-
```

---

## 🧠 Best Practices

- Use `workflow_dispatch` for manual triggers.
- Use reusable workflows for repeated patterns.
- Set up branch protection with required checks.
- Use caching to speed up builds.
- Keep secrets safe using GitHub Secrets.
- Use job matrix to test against multiple environments.

---

# 📎 Appendix: Reusable GitHub Actions Workflows & Examples

---

## 🧩 Reusable Workflow Template

Reusable workflows allow you to define a workflow once and call it from other workflows.

`.github/workflows/ci-template.yml`

```
name: CI Template

on:
  workflow_call:
    inputs:
      node-version:
        required: true
        type: string
    secrets:
      CUSTOM_TOKEN:
        required: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: ${{ inputs.node-version }}

      - run: npm ci
      - run: npm test
```

Then call it from another workflow like:

```
name: Reuse CI

on:
  push:
    branches: [main]

jobs:
  call-ci:
    uses: ./.github/workflows/ci-template.yml
    with:
      node-version: '18'
    secrets:
      CUSTOM_TOKEN: ${{ secrets.CUSTOM_TOKEN }}
```

---

## 🧪 Examples by Tech Stack

### 🔷 Node.js / TypeScript

```
name: Node CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [16, 18]
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm ci
      - run: npm run build
      - run: npm test
```

---

### 🐍 Python

```
name: Python CI

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: [3.8, 3.10]
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: ${{ matrix.python-version }}
      - run: pip install -r requirements.txt
      - run: pytest
```

---

### 🌐 React (or any front-end)

```
name: React CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm ci
      - run: npm run build
```

---

### 🟡 Docker Build & Push

```
name: Docker CI

on:
  push:
    branches: [main]

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build Docker image
        run: docker build -t myapp:latest .

      - name: Log in to DockerHub
        run: echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin

      - name: Push to DockerHub
        run: docker push myapp:latest
```

---

### ☁️ Deploy to Vercel

```
name: Deploy to Vercel

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          working-directory: ./
```

---

### 🔺 Terraform Plan & Apply

```
name: Terraform

on:
  push:
    branches: [main]

jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
      - uses: hashicorp/setup-terraform@v3
      - run: terraform init
      - run: terraform plan
      - run: terraform apply -auto-approve
        env:
          TF_VAR_api_key: ${{ secrets.API_KEY }}
```

---

## 🟦 Go (Golang)

```
name: Go CI

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Setup Go
        uses: actions/setup-go@v4
        with:
          go-version: '1.21'

      - name: Install dependencies
        run: go mod tidy

      - name: Build
        run: go build -v ./...

      - name: Run Tests
        run: go test -v ./...
```

---

## ☕ Java with Maven

```
name: Java Maven CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Set up JDK
        uses: actions/setup-java@v3
        with:
          java-version: '17'
          distribution: 'temurin'

      - name: Build with Maven
        run: mvn -B package --file pom.xml
```

---

## 💎 Ruby on Rails

```
name: Rails CI

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:14
        env:
          POSTGRES_USER: postgres
          POSTGRES_PASSWORD: password
        ports:
          - 5432:5432
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      - uses: actions/checkout@v3

      - uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.2'

      - name: Setup dependencies
        run: |
          gem install bundler
          bundle install

      - name: Set up database
        run: |
          cp config/database.yml.github-actions config/database.yml
          bundle exec rake db:create
          bundle exec rake db:schema:load

      - name: Run tests
        run: bundle exec rspec
```

---

## 🟣 Laravel (PHP)

```
name: Laravel CI

on:
  push:
    branches: [main]

jobs:
  laravel-tests:
    runs-on: ubuntu-latest
    services:
      mysql:
        image: mysql:8.0
        env:
          MYSQL_ROOT_PASSWORD: root
          MYSQL_DATABASE: testing
        ports:
          - 3306:3306
        options: >-
          --health-cmd "mysqladmin ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 3

    steps:
      - uses: actions/checkout@v3

      - name: Setup PHP
        uses: shivammathur/setup-php@v2
        with:
          php-version: '8.2'
          extensions: mbstring, bcmath, mysql

      - name: Install Composer dependencies
        run: composer install --no-progress --prefer-dist

      - name: Prepare Laravel Application
        run: |
          cp .env.example .env
          php artisan key:generate
          php artisan migrate --force

      - name: Run tests
        run: vendor/bin/phpunit
```

---
