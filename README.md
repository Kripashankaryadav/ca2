# Node.js Express Docker CI/CD Project

## Overview
Ye project ek simple Node.js Express application hai jo `GET /health` endpoint provide karta hai. Iske saath Docker support aur GitHub Actions CI/CD pipeline bhi banaaya gaya hai.

## Project structure
- `index.js`: Express server application
- `package.json`: Node project metadata and dependencies
- `Dockerfile`: Docker image banane ke liye configuration
- `.github/workflows/main.yaml`: GitHub Actions CI/CD workflow

## Step-by-step explanation

### 1. Application creation
- `index.js` mein Express app banaya gaya hai.
- `app.get('/health')` route server status return karta hai.
- `app.listen(3000)` se application port `3000` pe chalega.

Yeh step isliye zaroori hai kyunki humko ek working REST API chahiye thi jisko Docker aur CI pipeline se deploy/test kiya ja sake.

### 2. Dependencies setup
- `package.json` mein `express` dependency add ki gayi.
- `npm install` se local `node_modules` install hote hain.

Yeh step isliye hai taaki app docker build aur local run ke liye required packages mil jayein.

### 3. Dockerfile banaya
`Dockerfile` ke steps:
- `FROM node:18-alpine`: lightweight Node.js image use karta hai.
- `WORKDIR /app`: working directory set hoti hai.
- `COPY package*.json ./` aur `RUN npm install`: dependencies install hote hain.
- `COPY . .`: baaki project files container mein copy hoti hain.
- `EXPOSE 3000`: container port expose karta hai.
- `CMD ["node", "index.js"]`: container start hone par server run hota hai.

Yeh step isliye zaroori hai taaki application ko container image mein pack kar sakein.

### 4. GitHub Actions workflow
`.github/workflows/main.yaml` mein ek single pipeline create hua hai.

Workflow steps:
1. `Checkout Code`: GitHub repo se code leta hai.
2. `Setup Node`: Node 18 environment tayyar karta hai.
3. `Install Dependencies`: `npm install` chalata hai.
4. `Run Tests`: abhi placeholder hai (`echo "No tests yet"`).
5. `Build Docker Image`: `docker build -t my-api .` image banata hai.
6. `Test Docker Container`: image chalaata hai, health endpoint call karta hai, phir container stop/remove karta hai.
7. `Login to DockerHub`: DockerHub credentials se login hota hai. Yeh step tabhi chalega jab secrets set hon.
8. `Push Docker Image`: image ko DockerHub pe push karta hai.

Yeh workflow isliye banta hai taaki code push karte hi automatic build, test, aur image push ho jaye.

### 5. Git push and CI/CD trigger
- `git add .` aur `git commit -m "This is the new CI/CD pipeline for CA2"` commit create kiya.
- `git push origin main` repository ko GitHub par bheja.

Ab GitHub Actions `main` branch par push detect kar ke workflow execute karega.

## Local run commands
1. Install dependencies:
```bash
npm install
```
2. Run locally:
```bash
node index.js
```
3. Check endpoint:
```bash
curl http://localhost:3000/health
```
4. Build Docker image:
```bash
docker build -t my-api .
```
5. Run Docker container:
```bash
docker run -d --name my-container -p 3000:3000 my-api
```
6. Test container:
```bash
curl http://localhost:3000/health
```
7. Stop container:
```bash
docker stop my-container
docker rm my-container
```

## GitHub Actions notes
- GitHub pe `.github/workflows/main.yaml` hona zaroori hai.
- Workflow `push` event pe trigger hota hai, sirf `main` branch ke liye.
- DockerHub push ke liye `DOCKER_USERNAME` aur `DOCKER_PASSWORD` secrets GitHub repo settings mein add karne honge.

## CI/CD Explanation
- CI (Continuous Integration): Code push hone par automatic build aur test hota hai.
- CD (Continuous Deployment): Docker image build hone ke baad DockerHub par push hoti hai aur future deployment ke liye ready rehti hai.

## Deployment Note
Filhaal project DockerHub tak deploy karta hai. Isse aage is image ko cloud platforms jaise AWS, Render, ya Railway par run karke fully deployed application banaya ja sakta hai.

## Health Endpoint Response
Example response:
```json
{
  "status": "Server is running 🚀"
}
```

### Test Docker Container (CI step example)
```bash
docker run -d -p 3000:3000 my-api
sleep 5
curl http://localhost:3000/health
docker stop $(docker ps -q)
```

## CI/CD Flow
Code Push → GitHub Actions → Build → Test → Docker Image → DockerHub

## IMPORTANT check
👉 Ye line check kar:

`.github/workflows/main.yaml`

👉 Agar file `.yml` hai to change kar:

`.github/workflows/main.yml`

## Summary
Project ab complete hai:
- Application bana hua hai
- Docker image ready hai
- CI pipeline configured hai
- GitHub par code push ho chuka hai

Agar aap chaho, main ab `.github/workflows/main.yaml` mein `push` ke alawa `pull_request` bhi add kar sakta hoon, ya local tests likh sakta hoon.`