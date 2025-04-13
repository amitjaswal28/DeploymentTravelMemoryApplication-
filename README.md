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

![Screenshot 2025-04-13 202831](https://github.com/user-attachments/assets/c3b6d1c2-c51f-41f0-9eb1-d67ffa71b2f9)
![Screenshot 2025-04-13 202810](https://github.com/user-attachments/assets/f69257a0-1ad6-4d7e-93d9-a74c93c1206a)
![Screenshot 2025-04-13 202750](https://github.com/user-attachments/assets/34e2ac2f-c536-4ceb-9c9d-166c65e8e66b)
![Screenshot 2025-04-13 201828](https://github.com/user-attachments/assets/064b7f18-3e28-41eb-b98b-f95d031ecc31)
![Screenshot 2025-04-13 201055](https://github.com/user-attachments/assets/882182b1-b9e5-4187-8359-a41e9c1927e7)
![Screenshot 2025-04-13 201013](https://github.com/user-attachments/assets/2ac793e7-7832-4b4f-8971-435f4d226fa2)
![Screenshot 2025-04-13 200956](https://github.com/user-attachments/assets/d8f8f7bb-ee4f-4d23-8a0c-2c1a3ead4ef2)
![Screenshot 2025-04-13 200940](https://github.com/user-attachments/assets/9b74dbc7-f883-42e8-b565-5be172954a17)
![Screenshot 2025-04-13 200337](https://github.com/user-attachments/assets/2515e8a7-2708-4538-81a5-77fa8fe0841e)
![Screenshot 2025-04-13 194729](https://github.com/user-attachments/assets/f4a80d32-ea67-4679-a7a1-9b7d714c748b)
![Screenshot 2025-04-13 191109](https://github.com/user-attachments/assets/a61b667b-3b85-4aef-8d49-0f240a25a1b5)

![Screenshot 2025-04-13 192317](https://github.com/user-attachments/assets/4d094e4e-958b-456f-bf39-2488528896f9)

![Screenshot 2025-04-13 185641](https://github.com/user-attachments/assets/8c811a23-2998-43c5-925e-fcafbdae8785)

![Screenshot 2025-04-13 182606](https://github.com/user-attachments/assets/cb6752a5-1f33-48f8-8457-b19b51dd839c)

![Screenshot 2025-04-13 180225](https://github.com/user-attachments/assets/01c2a8f4-0a27-46b6-ac8f-d16603220ba8)

![Screenshot 2025-04-13 174212](https://github.com/user-attachments/assets/ffa35646-cda5-4c33-8c8d-e03439e8a289)
![Screenshot 2025-04-13 174136](https://github.com/user-attachments/assets/a6207339-d17a-4c92-a602-53e5b5d698c9)
































