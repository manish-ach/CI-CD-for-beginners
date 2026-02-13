# Docker Static Site with CI/CD Pipeline 🐳

A simple project to learn Docker containerization and GitHub Actions CI/CD.

---

## 📁 Project Structure

```
csr/
├── .github/
│   └── workflows/
│       └── ci-cd.yml    # GitHub Actions workflow
├── Dockerfile           # Docker build instructions
├── main.html            # Static HTML file (portfolio)
└── README.md            # You are here!
```

---

## 🚀 What This Project Does

1. **Containerizes** a static HTML file using Docker + Nginx
2. **Automatically builds** the Docker image on every push
3. **Tests** the container by running it and checking the response
4. **Deploys** to Docker Hub when changes are pushed to `main`

---

## 🔄 CI/CD Pipeline Overview

```
┌──────────────────┐
│   Push to GitHub │
└────────┬─────────┘
         │
         ▼
┌──────────────────────────────────────┐
│  BUILD JOB                           │
│  ├── Checkout code                   │
│  ├── Build Docker image              │
│  └── Test container (curl check)     │
└────────┬─────────────────────────────┘
         │
         ▼ (only on main branch)
┌──────────────────────────────────────┐
│  DEPLOY JOB                          │
│  ├── Login to Docker Hub             │
│  └── Push image with tags            │
│       • :latest                      │
│       • :commit-sha                  │
└──────────────────────────────────────┘
```

---

## 🛠️ Local Development

### Prerequisites
- [Docker](https://docs.docker.com/get-docker/) installed

### Build the image
```bash
docker build -t my-static-site .
```

### Run the container
```bash
docker run -d -p 8080:80 my-static-site
```

### View the site
Open http://localhost:8080 in your browser.

### Stop the container
```bash
docker stop $(docker ps -q --filter ancestor=my-static-site)
```

---

## ⚙️ GitHub Actions Setup

### Required Secrets

Make sure to add necessary env variables in the GitHub repo: **Settings → Secrets → Actions**

| Secret Name       | Description                  |
|-------------------|------------------------------|
| `DOCKER_USERNAME` | Your Docker Hub username     |
| `DOCKER_TOKEN`    | Docker Hub access token      |

### How to Get Docker Hub Token
1. Go to [hub.docker.com](https://hub.docker.com)
2. Click your profile → **Account Settings**
3. Go to **Personal Access Token** → **Generate new token**
4. Name it `github-actions` and copy the token

---

## 📖 Key Concepts Explained

### Dockerfile
```dockerfile
FROM nginx:alpine      # Base image (lightweight web server)
COPY main.html /usr/share/nginx/html/index.html  # Add our HTML
EXPOSE 80              # Document the port
```

### GitHub Actions Workflow

| Keyword | Purpose |
|---------|---------|
| `on:` | Triggers - when the workflow runs |
| `jobs:` | Groups of steps that run on a VM |
| `steps:` | Individual tasks in a job |
| `uses:` | Use a pre-built action |
| `run:` | Execute a shell command |
| `needs:` | Job dependency (run after another job) |
| `if:` | Conditional execution |
| `secrets.*` | Access encrypted secrets |

---

## 🧪 Testing the Pipeline

1. Make a change to `main.html`
2. Commit and push to `main`
3. Go to **Actions** tab in GitHub
4. Watch your pipeline run! ✨

---

## 📚 Next Steps

- [ ] Add HTML validation tests
- [ ] Set up staging environment
- [ ] Add Slack/Discord notifications
- [ ] Deploy to a cloud provider (AWS, DigitalOcean)

---

## 📝 License
None for now☺️
