# Web Dashboard Deployment

This guide walks through the process of deploying Matra's Next.js web dashboard.

## Prerequisites
- DigitalOcean account or other cloud provider
- Domain name for the dashboard
- Node.js v16+ and npm
- Next.js knowledge

## Development Setup
```bash
# Clone the repository
git clone https://github.com/your-org/matra-dashboard.git
cd matra-dashboard

# Install dependencies
npm install

# Configure environment variables
cp .env.example .env.local
# Edit .env.local with your API URLs and other configuration

# Run development server
npm run dev
```

## Build for Production
```bash
# Create optimized production build
npm run build

# Test the production build locally
npm run start
```

## Deployment Options

### Option 1: Vercel (Recommended)

1. Push your code to a Git repository (GitHub, GitLab, BitBucket)
2. Connect your repository to Vercel
3. Configure environment variables in the Vercel dashboard
4. Deploy the project

Vercel will automatically build and deploy your Next.js application, and provide preview deployments for pull requests.

### Option 2: DigitalOcean App Platform

1. Log in to your DigitalOcean account
2. Navigate to "Apps" and click "Create App"
3. Connect your Git repository
4. Select the repository and branch
5. Configure your app:
   - Type: Web Service
   - Build Command: `npm run build`
   - Run Command: `npm start`
6. Configure environment variables
7. Select a plan (Basic or Pro)
8. Launch the app

### Option 3: Traditional VPS Deployment

#### Set Up the Server
```bash
# Update packages
sudo apt-get update
sudo apt-get upgrade -y

# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install PM2 for process management
sudo npm install -g pm2

# Set up Nginx
sudo apt-get install nginx -y
```

#### Deploy the Dashboard
```bash
# Create a directory for the app
mkdir -p /var/www/matra-dashboard
cd /var/www/matra-dashboard

# Clone the repository or upload your files
git clone https://github.com/your-org/matra-dashboard.git .

# Install dependencies and build
npm install --production
npm run build

# Start the app with PM2
pm2 start npm --name "matra-dashboard" -- start
pm2 save
pm2 startup

# Configure Nginx as a reverse proxy
sudo nano /etc/nginx/sites-available/matra-dashboard
```

Nginx configuration:
```nginx
server {
    listen 80;
    server_name dashboard.yourdomain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Enable the site and set up SSL:
```bash
sudo ln -s /etc/nginx/sites-available/matra-dashboard /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx

# Set up SSL with Let's Encrypt
sudo apt-get install certbot python3-certbot-nginx -y
sudo certbot --nginx -d dashboard.yourdomain.com
```

## Continuous Deployment

### GitHub Actions Workflow
```yaml
name: Deploy to Production

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          vercel-args: '--prod'
```

## Performance Optimization
- Enable caching in Next.js configuration
- Implement Image Optimization with next/image
- Configure Content Security Policy headers
- Set up proper cache-control headers 