p26-multienv/
├── app/
│   ├── app.py               Flask app
│   ├── requirements.txt     flask
│   └── Dockerfile           builds the image
├── docker-compose.yml       local testing
└── .github/
    └── workflows/
        ├── deploy-dev.yml   dev branch → dev server
        └── deploy-prod.yml  main branch → staging → prod

  ------------------------------------------------------------
git push origin dev
        ↓
GitHub Actions triggers
        ↓
Docker image built + pushed to DockerHub
        ↓
SSH into Dev EC2
        ↓
Pull latest image + restart container
        ↓
App live on Dev server
  --------------------------------------------------------------
git push origin main
        ↓
GitHub Actions triggers
        ↓
Tests run → must pass
        ↓
Docker image built + pushed to DockerHub
        ↓
SSH into Prod EC2
        ↓
Pull latest image + restart container
        ↓
App live on Production
---------------------------------------------------------------
Lessons Learned

Always do it manually first — automation is just translating manual steps into yml
Read the exact error message — most bugs are one Google search away
|| true makes docker stop/rm idempotent — works on first deploy too
Security groups must allow port 22 for GitHub Actions runner to SSH in
