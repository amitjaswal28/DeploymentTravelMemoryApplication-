# 🌍 TravelMemory - Full Stack Deployment (EC2, NGINX, Load Balancer)
Note: not done CloudFlare because i dont have domain.
This guide documents the complete step-by-step deployment of the TravelMemory MERN Stack application to Amazon EC2, including backend and frontend setup, reverse proxy configuration, process management, and load balancing.

---

## ✅ Project Overview

- **App Name:** TravelMemory
- **Stack:** MERN (MongoDB, Express, React, Node.js)
- **Hosting:** AWS EC2 with Load Balancer
- **Process Management:** PM2
- **Web Server:** NGINX
- **Domain:** Optional (Access via IP or Load Balancer DNS)

---

## 🔧 Steps Performed

### 1. 🚀 Launch EC2 Instance

- Launched an Ubuntu EC2 instance using AWS Console.
- Selected existing key pair (`AmitJ-Key.pem`) for SSH access.
- Configured Security Group to allow:
  - **22** (SSH)
  - **80** (HTTP)
  - **443** (HTTPS)
  - **3000** (Node backend)
  - **5173** (React frontend dev)
  - **3001** (optional second instance)

---

### 2. 🔐 SSH Access

```bash
chmod 400 AmitJ-Key.pem
ssh -i "AmitJ-Key.pem" ubuntu@<EC2_PUBLIC_IP>
3. 📦 System & Stack Setup
bash
Copy
Edit
sudo apt update && sudo apt upgrade -y
sudo apt install -y nginx git curl unzip
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
sudo npm install -g pm2
4. 📥 Clone and Set Up the Project
bash
Copy
Edit
git clone https://github.com/UnpredictablePrashant/TravelMemory.git
cd TravelMemory
5. ⚙️ Backend Configuration
bash
Copy
Edit
cd backend
npm install

# .env file setup
cat <<EOF > .env
PORT=3000
MONGO_URI=<Your Mongo URI>
JWT_SECRET=<Your Secret>
EOF

# Start with PM2
pm2 start index.js --name travel-backend
cd ..
6. 🧑‍💻 Frontend Setup
bash
Copy
Edit
cd frontend
npm install

# Update base URL in src/urls.js
# Example:
# export const BASE_URL = "http://<your-backend-ec2-ip-or-lb-dns>/api";

npm run build
sudo cp -r dist/* /var/www/html/
7. 🌐 NGINX Reverse Proxy Setup
bash
Copy
Edit
sudo nano /etc/nginx/sites-available/backend
Paste the following:

nginx
Copy
Edit
server {
    listen 80;
    server_name <your-ec2-public-ip>;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
Enable and reload:

9. 🔁 Load Balancer Setup
Go to AWS EC2 → Load Balancers → Create Load Balancer

Choose Application Load Balancer

Create 2 Target Groups:

Frontend group (port 80)

Backend group (port 3000)

Register appropriate EC2 instances with each group

Add listeners and rules to forward traffic

10. 🧪 Testing
Access frontend via Load Balancer DNS:

cpp
Copy
Edit
http://<frontend-lb-dns>
Access backend/API:

perl
Copy
Edit
http://<backend-lb-dns>/api/trip
🧼 PM2 Management


A Record (for frontend EC2 IP)

CNAME (for Load Balancer DNS)

But domain setup is not mandatory.

📬 Deliverables
✅ Fully deployed TravelMemory App

📷 Screenshots of setup & Load Balancer

📝 This README for reference

🔗 GitHub Repo with all updates


![image](https://github.com/user-attachments/assets/729cc463-a77a-4f22-af18-0b976b5462e5)
![image](https://github.com/user-attachments/assets/c0599983-c9df-42eb-923a-7b93c54e1888)
![image](https://github.com/user-attachments/assets/2feb9455-2e90-4cc2-a7ed-f6c92dec9d7c)
![image](https://github.com/user-attachments/assets/f4004cf7-4c83-4ecf-a909-73feeae28bc6)
![image](https://github.com/user-attachments/assets/d50237b0-37c6-4d05-8472-cc50b891923f)
![image](https://github.com/user-attachments/assets/a1155fb7-d0b1-4ad6-ac7c-af75376cb2bb)
![image](https://github.com/user-attachments/assets/21dbf3ff-05ec-4690-9bfe-c476dfb5d1ec)
![image](https://github.com/user-attachments/assets/46111388-94f9-4bb3-b8a2-e25322dd91bc)
![image](https://github.com/user-attachments/assets/fc6b630f-8283-46ba-b8fa-882d404a9c88)

![image](https://github.com/user-attachments/assets/7df9faed-a98d-451c-806d-7884c10de67a)

































