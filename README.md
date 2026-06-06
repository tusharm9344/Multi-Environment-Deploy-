Infrastructure Note
The EC2 instances provisioned for this project have been torn down after successful testing to avoid ongoing AWS costs. The pipeline was 
fully functional — both dev and production deployments were verified end to end before teardown. To reproduce, launch two Ubuntu 22.04
EC2 instances on AWS, open ports 22 and 5000 in the security group, install Docker, and add the required GitHub Secrets listed below.
==============================================
P-26 — Multi-Environment Deploy
Automated CI/CD pipeline that deploys to different servers based on which Git branch you push to.
How It Works ->
Dev Pipeline
        When code is pushed to the dev branch, GitHub Actions builds a Docker image and pushes it to DockerHub. It then SSHes into the dev 
        EC2 instance, pulls the latest image, and restarts the container.
Prod Pipeline ->
        When code is pushed to main, GitHub Actions first runs tests. If tests pass, it builds the Docker image, pushes to DockerHub,
        SSHes into the production EC2 instance, pulls the image, and restarts the container. If tests fail, the deployment is blocked.
        
===============================================        
        GitHub Secrets Required
Add these under Repo - Settings - Secrets and variables - Actions.

DOCKER_USERNAME — DockerHub username
DOCKER_PASSWORD — DockerHub password
DEV_HOST — Dev EC2 public IP
PROD_HOST — Prod EC2 public IP
SSH_PRIVATE_KEY — Full content of EC2 .pem key file
================================================

Key Concepts
Branch-based triggers — different branches run different pipelines.
Job dependencies using needs — the production job only runs if the test job passes first.
GitHub Secrets — no credentials hardcoded anywhere in the codebase.
DockerHub as shared registry — the GitHub runner builds and pushes the image, EC2 pulls and runs it. Both are separate machines with no 
direct connection.
Smoke testing — curl verifies the app is actually running after deploy before the pipeline marks success.
=================================================
Lessons Learned
Always do the deployment manually first before writing any automation. The yml file is just those same manual commands written in a 
structured format. Read error messages carefully — most issues are solved with one Google search. The security group must allow port 22 
inbound for GitHub Actions to SSH into EC2.
