# Backend Deployment (DigitalOcean)

This internal documentation describes our backend infrastructure and deployment process on DigitalOcean.

## Infrastructure Overview
- **Server Type**: DigitalOcean VPS Droplets
- **Operating System**: Ubuntu 20.04 LTS
- **Container Platform**: Docker & Docker Compose
- **Database**: MongoDB Atlas
- **Load Balancer**: DigitalOcean Load Balancer

## Server Specifications
- Production: 4 CPU, 8GB RAM droplets ($40/month)
- Staging: 2 CPU, 4GB RAM droplet ($20/month)
- Development: 1 CPU, 2GB RAM droplet ($10/month)

## Server Setup Process

The following represents our standard server setup process:

```bash
# Initial server updates
sudo apt-get update
sudo apt-get upgrade -y

# Docker installation
sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io -y

# Docker Compose installation
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Security configuration
sudo ufw allow OpenSSH
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```

## Deployment Architecture

Our backend services are deployed as Docker containers:

```
api.matra.io --> Load Balancer --> API Containers (3 instances)
                                   |
                                   v
                                MongoDB Atlas (Cloud Database)
                                   |
                                   v
                                Redis Cache
```

## Deployment Process

Our deployment workflow uses GitHub Actions:

```yaml
name: Deploy to Production

on:
  workflow_dispatch:
  push:
    branches: [main]
    paths:
      - 'backend/**'

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v1
        
      - name: Login to Container Registry
        uses: docker/login-action@v1
        with:
          registry: registry.digitalocean.com
          username: ${{ secrets.DIGITALOCEAN_ACCESS_TOKEN }}
          password: ${{ secrets.DIGITALOCEAN_ACCESS_TOKEN }}
          
      - name: Build and push
        uses: docker/build-push-action@v2
        with:
          context: ./backend
          push: true
          tags: registry.digitalocean.com/matra/api:latest
          
      - name: Deploy to DigitalOcean
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.DO_HOST }}
          username: ${{ secrets.DO_USERNAME }}
          key: ${{ secrets.DO_KEY }}
          script: |
            cd /opt/matra
            docker-compose pull
            docker-compose up -d
```

## Database Management

MongoDB Atlas is used for database hosting with the following configuration:
- Production: M20 dedicated cluster with 3 nodes
- Staging: M10 dedicated cluster
- Development: M0 shared cluster

## SSL Configuration

SSL certificates are managed through Let's Encrypt:

```bash
# Install Certbot
sudo apt-get install certbot python3-certbot-nginx -y

# Obtain SSL certificate
sudo certbot --nginx -d api.matra.io
```

## Backup Strategy
- Database: Automated daily snapshots retained for 30 days
- Server: Weekly VM snapshots retained for 14 days
- Code: GitHub repository with protected branches

## Monitoring Setup
- Server metrics: DigitalOcean Monitoring
- Application performance: New Relic
- Error tracking: Sentry
- Logs: Papertrail 