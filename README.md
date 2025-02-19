```markdown
# FastAPI Beyond CRUD

This is the source code for the [FastAPI Beyond CRUD](https://youtube.com/playlist?list=PLEt8Tae2spYnHy378vMlPH--87cfeh33P&si=rl-08ktaRjcm2aIQ) course. The course focuses on FastAPI development concepts that go beyond the basic CRUD operations.

For more details, visit the project's [website](https://jod35.github.io/fastapi-beyond-crud-docs/site/).

## Table of Contents

1. [Getting Started](#getting-started)
2. [Prerequisites](#prerequisites)
3. [Project Setup](#project-setup)
4. [Running the Application](#running-the-application)
5. [Running Tests](#running-tests)
6. [GitHub Actions & Automation](#gitHub-actions--automation)
7. [Setting Up Environment Variables & Secrets](#setting-up-environment-variables--secrets)
8. [Contributing](#contributing)

---

## Getting Started

Follow the instructions below to set up and run your FastAPI project.

### Prerequisites

Ensure you have the following installed:

- Python **>= 3.10**
- PostgreSQL
- Redis
- **Docker & Docker Compose** (for containerized deployment)

---

## Project Setup

1. **Clone the project repository:**
   ```bash
   git clone https://github.com/jod35/fastapi-beyond-CRUD.git
   ```

2. **Navigate to the project directory:**
   ```bash
   cd fastapi-beyond-CRUD/
   ```

3. **Create and activate a virtual environment:**
   ```bash
   python3 -m venv env
   source env/bin/activate
   ```

4. **Install the required dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

5. **Set up environment variables:**
   - Copy `.env.example` and rename it to `.env`
   - Open the `.env` file and add your credentials (database, email, JWT secrets, etc.)
   
   ```bash
   cp .env.example .env
   ```

6. **Run database migrations to initialize the database schema:**
   ```bash
   alembic upgrade head
   ```
   *Note:* The `entrypoint.sh` script runs database migrations automatically when using **Docker Compose**.

7. **Start the Celery worker** (Linux/Unix shell):
   ```bash
   sh runworker.sh
   ```

---

## Running the Application

### Option 1: Running locally

Start the application:
```bash
fastapi dev src/
```

### Option 2: Running with Docker Compose

- **Before** running `docker compose up`, ensure your **`.env`** file is properly configured with your credentials.
- Then, start the application:
  ```bash
  docker compose up -d
  ```
- This will launch **FastAPI**, **PostgreSQL**, **Redis**, and **Celery workers** in separate containers.

---

## Running Tests

Run the tests using:
```bash
pytest
```
The `pytest` dependency was missing in `requirements.txt`, so it was added to prevent errors during testing.

---

## GitHub Actions & Automation

### Nightly Builds & Docker Deployment

- A **GitHub Actions workflow** (`nightlyBuild.yml`) automates testing and deployment.
- It **runs unit tests** and **sends an email notification** in case of failures.
- If successful, it **builds and pushes Docker images** to **Docker Hub**.
- The **Docker credentials token** was initially set to **read-only**, preventing image pushes. It was updated to **read, write, and delete** to enable automation.

### Conventional Commits Enforcement

- A **GitHub Actions workflow** (`conventional_commit.yml`) ensures commit message standards for the **main branch** and pull requests to the main branch.
- This workflow:
  - **Verifies commit messages** using `webiny/action-conventional-commits@v1.3.0`
  - **Labels invalid PRs** with `"invalid-commit"`
  - **Sends an email notification** if the commit message format is incorrect
  - **Automatically closes PRs** that do not follow the conventional commit rules

### Time Zone Adjustments

- GitHub Actions **runs on UTC** by default.
- The **scheduled time** was adjusted to **Pacific Standard Time (PST)** for accurate automation timing.

---

## Setting Up Environment Variables & Secrets

### For Local Development

Ensure the `.env` file contains:

```plaintext
DATABASE_URL=<your_postgres_url>
JWT_SECRET=<your_secret_key>
JWT_ALGORITHM=HS256
REDIS_URL=<your_redis_url>
MAIL_USERNAME=<your_smtp_username>
MAIL_PASSWORD=<your_smtp_password>
MAIL_SERVER=smtp.ethereal.email
MAIL_PORT=587
MAIL_FROM=example@email.com
MAIL_FROM_NAME=YourApp
DOMAIN=yourdomain.com
```

### For GitHub Actions (Secrets Setup)

- Navigate to **GitHub Repo → Settings → Secrets → Actions**
- Add the following **secrets**:
  - `DATABASE_URL`
  - `JWT_SECRET`
  - `JWT_ALGORITHM`
  - `REDIS_URL`
  - `MAIL_USERNAME`
  - `MAIL_PASSWORD`
  - `MAIL_SERVER`
  - `MAIL_PORT`
  - `MAIL_FROM`
  - `MAIL_FROM_NAME`
  - `DOMAIN`
  - **Docker Hub Credentials**:
    - `DOCKERHUB_USERNAME`
    - `DOCKERHUB_TOKEN` (set to **read, write, and delete**)

---

## Contributing

I welcome contributions to improve the documentation and the project! You can contribute [here](https://github.com/jod35/fastapi-beyond-crud-docs).
