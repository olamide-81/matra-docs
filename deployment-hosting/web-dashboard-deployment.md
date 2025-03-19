# Web Dashboard Deployment

This internal documentation describes our infrastructure and deployment process for the Matra web dashboard.

## Technology Stack
- **Frontend Framework**: Next.js
- **Hosting**: DigitalOcean Droplets
- **Build Tool**: GitHub Actions
- **Containerization**: Docker
- **CDN**: CloudFlare

## Environment Configuration
- **Development**: Local Next.js development server
- **Staging**: DigitalOcean droplet (2GB RAM)
- **Production**: DigitalOcean droplet (4GB RAM)

## Local Development Setup
```bash
# Setup development environment
git clone git@github.com:matra/web-dashboard.git
cd web-dashboard

# Install dependencies
npm install

# Run development server
npm run dev
```

## Build Process
```bash
# Create optimized production build
npm run build

# Start production server
npm start
```

## Production Deployment Architecture

The web dashboard uses a Docker container deployed on a DigitalOcean droplet with Nginx:

```
web.matra.io --> CloudFlare --> Nginx --> Next.js Container
```

## Deployment Process

Our team uses GitHub Actions for automated deployments:

```yaml
name: Deploy Web Dashboard

on:
  workflow_dispatch:
  push:
    branches: [main]
    paths:
      - 'web-dashboard/**'

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Setup Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '16'
          
      - name: Install dependencies
        run: npm ci
        working-directory: ./web-dashboard
        
      - name: Build Next.js application
        run: npm run build
        working-directory: ./web-dashboard
        env:
          NEXT_PUBLIC_API_URL: ${{ secrets.NEXT_PUBLIC_API_URL }}
          
      - name: Build and push Docker image
        uses: docker/build-push-action@v2
        with:
          context: ./web-dashboard
          push: true
          tags: registry.digitalocean.com/matra/web-dashboard:latest
          
      - name: Deploy to DigitalOcean
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.WEB_HOST }}
          username: ${{ secrets.WEB_USERNAME }}
          key: ${{ secrets.WEB_KEY }}
          script: |
            cd /opt/matra-web
            docker-compose pull
            docker-compose up -d
```

## Server Configuration

Our DigitalOcean droplet is configured with the following:

```bash
# Update system packages
sudo apt update && sudo apt upgrade -y

# Install Docker
sudo apt install docker.io docker-compose -y

# Create application directory
sudo mkdir -p /opt/matra-web
cd /opt/matra-web

# Create docker-compose.yml
cat > docker-compose.yml << 'EOF'
version: '3'
services:
  web:
    image: registry.digitalocean.com/matra/web-dashboard:latest
    restart: always
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
  nginx:
    image: nginx:latest
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/conf.d:/etc/nginx/conf.d
      - ./certbot/conf:/etc/letsencrypt
      - ./certbot/www:/var/www/certbot
    depends_on:
      - web
EOF

# Set up Nginx configuration
mkdir -p nginx/conf.d
cat > nginx/conf.d/default.conf << 'EOF'
server {
    listen 80;
    server_name web.matra.io;
    
    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }
    
    location / {
        return 301 https://$host$request_uri;
    }
}

server {
    listen 443 ssl;
    server_name web.matra.io;
    
    ssl_certificate /etc/letsencrypt/live/web.matra.io/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/web.matra.io/privkey.pem;
    
    location / {
        proxy_pass http://web:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
EOF
```

## SSL Certificate Automation

We use Certbot for SSL certificate management:

```bash
# Install Certbot
sudo apt-get install certbot python3-certbot-nginx -y

# Set up initial certificate
sudo certbot --nginx -d web.matra.io

# Create renewal script
cat > /etc/cron.weekly/renew-certs << 'EOF'
#!/bin/bash
certbot renew --quiet
docker-compose -f /opt/matra-web/docker-compose.yml restart nginx
EOF

# Make script executable
chmod +x /etc/cron.weekly/renew-certs
```

## Monitoring and Logging

We use the following tools for monitoring:
- **Uptime monitoring**: UptimeRobot
- **Error tracking**: Sentry
- **Performance monitoring**: Vercel Analytics
- **Logs**: Papertrail

## Backup Strategy
- Daily DigitalOcean backups
- Full system snapshots retained for 7 days
- Code repositories backup on GitHub and GitLab 