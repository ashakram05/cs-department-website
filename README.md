# 🖥️ CS Department Website — DevOps Lab Assignment

A static departmental website for the Computer Science Department, fully containerized and deployed using a multi-environment CI/CD pipeline.

---

## 📁 Project Structure

```
cs-department-website/
├── index.html              # Home Page (Team Lead)
├── courses.html            # Courses Page
├── faculty.html            # Faculty Page
├── admissions.html         # Admissions Page
├── contact.html            # Contact Page
├── style.css               # Shared stylesheet
├── Dockerfile              # Docker containerization
├── nginx.conf              # Nginx web server config
├── .htmlhintrc             # HTML linting rules
├── .gitignore
├── README.md
└── .github/
    └── workflows/
        ├── ci.yml          # CI: lint + Docker build
        ├── cd-dev.yml      # CD: deploy to Development
        ├── cd-staging.yml  # CD: deploy to Staging/QA
        └── cd-prod.yml     # CD: deploy to Production
```

---

## 🌿 Git Flow Branch Strategy

| Branch        | Purpose                          | Deploys To   |
|---------------|----------------------------------|--------------|
| `develop`     | Active development, feature merges | Development |
| `release/*`   | Pre-release QA testing            | Staging/QA  |
| `main`        | Stable production-ready code      | Production  |

### Workflow:
1. Each member creates a feature branch: `feature/your-name-page`
2. Open a PR → merge into `develop` → triggers Dev deployment
3. When ready for QA, create `release/v1.x` branch → triggers Staging deployment
4. After QA approval, merge `release/v1.x` into `main` → triggers Production deployment

---

## 🐳 Docker

Build and run locally:

```bash
# Build image
docker build -t cs-department-website .

# Run container
docker run -p 8080:80 cs-department-website

# Visit http://localhost:8080
```

---

## ⚙️ GitHub Actions Pipelines

### CI Pipeline (`ci.yml`)
Triggered on every push/PR to `develop`, `release/*`, `main`:
- ✅ HTMLHint linting on all `.html` files
- ✅ Stylelint on all `.css` files
- ✅ Docker image build (validates Dockerfile)

### CD Pipelines
| Workflow         | Trigger Branch  | Environment  |
|------------------|-----------------|--------------|
| `cd-dev.yml`     | `develop`        | Development  |
| `cd-staging.yml` | `release/**`     | Staging/QA   |
| `cd-prod.yml`    | `main`           | Production   |

---

## 🔐 GitHub Secrets Setup

For each GitHub Environment (`development`, `staging`, `production`), add these secrets:

| Secret Name               | Description                          |
|---------------------------|--------------------------------------|
| `RENDER_SERVICE_ID_DEV`   | Render service ID for Dev            |
| `RENDER_DEPLOY_KEY_DEV`   | Render deploy hook key for Dev       |
| `RENDER_SERVICE_ID_STAGING` | Render service ID for Staging      |
| `RENDER_DEPLOY_KEY_STAGING` | Render deploy hook key for Staging |
| `RENDER_SERVICE_ID_PROD`  | Render service ID for Production     |
| `RENDER_DEPLOY_KEY_PROD`  | Render deploy hook key for Production|

---

## 🚀 Render.com Deployment Setup

1. Go to [render.com](https://render.com) → New → **Web Service**
2. Connect your GitHub repo
3. Set:
   - **Environment**: Docker
   - **Branch**: `develop` (for Dev), `release/v1.0` (for Staging), `main` (for Prod)
4. After creation, go to **Settings → Deploy Hook** → copy the URL
5. Extract `srv-XXXX` (Service ID) and the `key=XXXX` from the deploy hook URL
6. Add them as GitHub Secrets under the appropriate Environment

---



---

## 🛡️ Branch Protection Rules (Recommended)

Set on `main` and `develop`:
- Require pull request before merging
- Require at least 1 reviewer approval
- Require status checks (CI) to pass before merging
- Restrict direct pushes to protected branches
