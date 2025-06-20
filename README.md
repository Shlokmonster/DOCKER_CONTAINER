
#  Vite Frontend - Dockerized & CI/CD Ready

This is a production-ready frontend built with [Vite](https://vitejs.dev/), fully containerized using Docker and deployed via GitHub Actions to Docker Hub.

##  Tech Stack

-  [Vite](https://vitejs.dev/) (React)
- Docker (multi-stage)
-  GitHub Actions (CI/CD pipeline)
-  Docker Hub (image hosting)
-  Render (optional deployment)

---

##  Folder Structure

```
.
├── frontend/
│   ├── Dockerfile       # Production-ready Dockerfile
│   ├── src/             # React components
│   ├── public/
│   ├── package.json
│   └── vite.config.js
├── .github/
│   └── workflows/
│       └── deploy.yml   # GitHub Actions CI/CD config
└── README.md
```

---

## Docker Image (Production)

The Dockerfile builds and serves the Vite app using `serve`.

```Dockerfile
# Build stage
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Serve stage
FROM node:18
RUN npm install -g serve
WORKDIR /app
COPY --from=builder /app/dist ./dist
EXPOSE 3000
CMD ["serve", "-s", "dist", "-l", "3000"]
```

---

## Running Locally

```bash
# Build the image
docker build -t shlokkadam/vite-app ./frontend

# Run it
docker run -p 3000:3000 shlokkadam/vite-app
```

Open [http://localhost:3000](http://localhost:3000)

---

##  CI/CD Setup (GitHub Actions)

Whenever you push to `main`, GitHub Actions:

1. Builds the Docker image from `frontend/`
2. Tags it as `latest`
3. Pushes it to Docker Hub (`shlokkadam/vite-app`)

**YAML location:** `.github/workflows/deploy.yml`

```yaml
run: docker build -t ${{ secrets.IMAGE_NAME }} ./frontend
run: docker push ${{ secrets.IMAGE_NAME }}:latest
```

---

##  Required GitHub Secrets

| Secret Name        | Description                          |
|--------------------|--------------------------------------|
| `DOCKER_USERNAME`  | Your Docker Hub username             |
| `DOCKER_PASSWORD`  | Docker Hub access token              |
| `IMAGE_NAME`       | e.g. `shlokkadam/vite-app`           |
| `RENDER_DEPLOY_HOOK` | (Optional) Auto-deploy to Render after push |

---

##  Deploying on Render (Optional)

Use your Dockerfile to deploy as a **Docker web service** on [Render.com](https://render.com). Add a deploy hook and trigger it via GitHub Actions:

```yaml
- name: Deploy to Render
  run: curl -X POST ${{ secrets.RENDER_DEPLOY_HOOK }}
```

---


---

## 🧠 Author

**Shlok Kadam**  
 Engineering student @ ITM Skills University  
 [GitHub](https://github.com/shlokkadam)

---

##  License

MIT — feel free to fork, deploy, and build on top of it!
