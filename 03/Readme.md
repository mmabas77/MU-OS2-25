### **🚀 Step-by-Step Guide: Setting Up Ubuntu 20 Server on VirtualBox for Node.js App**  
This guide will help you **install Ubuntu 20 Server on VirtualBox**, configure **bridged networking**, set up **SSH access**, install **Node.js & npm**, clone your **GitHub repo**, and finally **run your server & access it from Windows**.

---

## **1️⃣ Install Ubuntu 20 Server on VirtualBox**
1. **Download Ubuntu 20 Server**  
   - Download from [Ubuntu official site](https://releases.ubuntu.com/20.04/).

2. **Create a New Virtual Machine in VirtualBox**  
   - Open **VirtualBox** → Click **New**.
   - Name: **Ubuntu20Server**
   - Type: **Linux**
   - Version: **Ubuntu (64-bit)**
   - RAM: **2048MB (2GB) or more**.
   - **Create a Virtual Hard Disk** → 20GB+.

3. **Attach the Ubuntu ISO & Install Ubuntu Server**  
   - **Start the VM** → Select the **Ubuntu ISO**.
   - Follow the **installation wizard**.
   - **Set up a user** and **enable OpenSSH Server** (important for SSH access later).
   - Finish installation & restart.

---

## **2️⃣ Configure VirtualBox Network (Bridged Mode)**
1. **Shut down the Ubuntu VM**.
2. **Go to VirtualBox settings** → Select **Network**.
3. Change **Adapter 1** to:
   - **Attached to:** `Bridged Adapter`
   - **Allow All**
4. **Restart the VM**.

---

## **3️⃣ Get the Ubuntu Server IP Address**
Once the server is running, get the **local network IP**:
```bash
ip a | grep inet
```
Or 
```bash
hostname -I
```

Look for an IP like `192.168.1.xxx`. This is your Ubuntu server’s IP.

---

## **4️⃣ Connect to Ubuntu via SSH from Windows**
From your Windows **Command Prompt (cmd)**, run:
```bash
ssh username@192.168.1.xxx
```
- Replace `username` with your Ubuntu user.
- Replace `192.168.1.xxx` with your actual server IP.

---

## **5️⃣ Install Node.js & npm on Ubuntu**
On your Ubuntu server, run:
```bash
# Update package list (not very important)
sudo apt update && sudo apt upgrade -y

# Install Node.js & npm
sudo apt install nodejs npm -y

# Check versions
node -v
npm -v
```

---

## **6️⃣ Clone the GitHub Repository**
Navigate to the directory where you want to clone the repo:
```bash
cd ~  # Go to home directory

# Install Git (if not installed) (Likely installed)
sudo apt install git -y

# Clone your repository
git clone https://github.com/mmabas77/bookstore.git

# Change into project directory
cd bookstore
```

---

## **7️⃣ Install Dependencies & Run the Server**
Inside the cloned repo:
```bash
npm install  # Install project dependencies
node ./server.js  # Start the Node.js server
```
Your server should now be running on **port 3000**.

---

## **8️⃣ Add the Hostname to Windows Hosts File**
1. Open **Notepad as Administrator**.
2. Open the file:
   ```
   C:\Windows\System32\drivers\etc\hosts
   ```
3. Add this line at the bottom:
   ```
   192.168.1.xxx    bookstore.test
   ```
   Replace `192.168.1.xxx` with your **Ubuntu server’s IP**.

4. Save & close the file.
5. Flush DNS cache:
   ```bash
   ipconfig /flushdns
   ```

---

## **9️⃣ Access the Node.js App in Your Browser**
Now, open your browser and enter:
```
http://bookstore.test:3000/books
```
OR, if `bookstore.test` doesn't work, try:
```
http://192.168.1.xxx:3000/books
```

---

## **🎯 Final Checklist**
✔ **Ubuntu Server installed in VirtualBox**  
✔ **Bridged networking enabled**  
✔ **Connected to server via SSH**  
✔ **Installed Node.js, npm, and cloned the repo**  
✔ **Started the server with `node server.js`**  
✔ **Added hostname in Windows hosts file**  
✔ **Accessed the app via browser on port 3000**  

---

### **❓ Troubleshooting**
💡 **Cannot SSH into Ubuntu?**
- Check if SSH is installed: `sudo systemctl status ssh`
- If not running, start it: `sudo systemctl start ssh`
- Restart VirtualBox if needed.

💡 **Cannot access `bookstore.test`?**
- Try using the raw IP: `http://192.168.1.xxx:3000`
- Make sure `server.js` is running.

💡 **Port 3000 not accessible?**
- Check firewall: `sudo ufw allow 3000/tcp`
- Restart Node.js server.

🚀 **Let me know if you need more help!**