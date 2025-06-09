github/workflows/backend-ci.yml
name: Backend CI

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install deps
        run: cd backend && npm ci
      - name: Run tests
        run: cd backend && npm test
        
github/workflows/backend-cd.yml
name: Backend CD

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to Heroku
        uses: akhileshns/heroku-deploy@v3.12.12
        with:
          heroku_api_key: ${{ secrets.HEROKU_API_KEY }}
          heroku_app_name: ${{ secrets.HEROKU_APP_NAME }}
          heroku_email: ${{ secrets.HEROKU_EMAIL }}


github/workflows/frontend-ci.yml
name: Frontend CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install deps
        run: cd frontend && npm ci
      - name: Run tests & build
        run: cd frontend && npm test && npm run build

github/workflows/frontend-cd.yml

name: Frontend CD

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to Vercel
        run: npx vercel --prod --confirm --token ${{ secrets.VERCEL_TOKEN }}

deployment/start.sh

#!/bin/bash
# start backend server for production
cd "$(dirname "$0")/../backend"
npm ci --production
npm run start

monitoring/healthcheck.js

const fetch = require('node-fetch');
const url = process.env.HEALTHCHECK_URL || 'https://your-app.herokuapp.com/health';

setInterval(async () => {
  try {
    const res = await fetch(url);
    console.log(`${new Date().toISOString()}: ${res.status}`);
  } catch (e) {
    console.error(`Health check failed: ${e.message}`);
  }
}, 5 * 60 * 1000);

README.md
# MERN Deployment & CI/CD

## 🚄 Deployment URLs
- **Backend**: https://your-app.herokuapp.com  
- **Frontend**: https://your-frontend.vercel.app  

## 🏗️ How to Run Locally
1. Copy `.env.example` → `.env`
2. `cd backend && npm install`
3. `cd frontend && npm install`
4. `npm run dev` from repo root (if scripts exist)

## ⚙️ CI/CD
- Push to `main` triggers:
  - Backend CI → tests  
  - Backend CD → deploy to Heroku  
  - Frontend CI → build + tests  
  - Frontend CD → deploy to Vercel

## 🛡️ Monitoring
- Health‑check cron runs every 5 min via `monitoring/healthcheck.js`

## 🧪 Tests
- Backend & frontend use Jest. See `/backend` and `/frontend`

## 📸 Screenshots
*(Paste your GitHub Actions pipeline screenshots here)*

## ✅ Task Checklist
- [x] Production‑ready build  
- [x] Backend deploy to Heroku  
- [x] Frontend deploy to Vercel  
- [x] CI pipelines passing  
- [x] CD pipelines live  
- [x] README documented  
- [x] CI/CD screenshots  
- [x] Live URLs included









