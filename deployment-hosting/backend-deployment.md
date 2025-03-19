# Backend Deployment (DigitalOcean)

This guide walks through the deployment of Matra's backend on DigitalOcean.

## Prerequisites
- DigitalOcean account
- Domain name (for API endpoints)
- Docker installed locally
- Node.js v16+ and npm

## Deployment Steps

### 1. Create a Droplet
1. Log in to your DigitalOcean account
2. Navigate to "Droplets" and click "Create Droplet"
3. Select Ubuntu 20.04 LTS
4. Choose a plan (recommended: Basic, $20/mo with 4GB RAM)
5. Select a datacenter region closest to your target users
6. Add your SSH keys
7. Create the droplet

### 2. Configure the Server
```bash
# Update packages
sudo apt-get update
sudo apt-get upgrade -y

# Install Docker
sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io -y

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Set up firewall
sudo ufw allow OpenSSH
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```

### 3. Configure DNS
1. Add an A record in your domain's DNS settings pointing to your droplet's IP address
2. Set up reverse proxy (Nginx) to route traffic

### 4. Deploy the Backend
```bash
# Clone the repository
git clone https://github.com/your-org/matra-backend.git
cd matra-backend

# Configure environment variables
cp .env.example .env
nano .env  # Edit with your configuration

# Build and start the containers
docker-compose build
docker-compose up -d
```

### 5. Set Up SSL with Let's Encrypt
```bash
# Install Certbot
sudo apt-get install certbot python3-certbot-nginx -y

# Obtain SSL certificate
sudo certbot --nginx -d api.yourdomain.com
```

### 6. Configure MongoDB
```bash
# Secure MongoDB installation
docker exec -it matra-mongodb mongo

# In MongoDB shell
use admin
db.createUser({
  user: "adminUser",
  pwd: "securePassword",
  roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
})
exit

# Edit docker-compose.yml to enable MongoDB authentication
nano docker-compose.yml
```

### 7. Set Up Continuous Deployment
1. Configure GitHub Actions or similar CI/CD pipeline
2. Set up automated testing
3. Configure deployment scripts

## Scaling Considerations
- Set up multiple API nodes behind a load balancer
- Configure database replication for MongoDB
- Implement caching using Redis
- Set up monitoring with Prometheus and Grafana 