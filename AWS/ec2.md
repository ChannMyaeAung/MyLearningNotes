# EC2 Setup Instructions

1. Launch instance on AWS Console. Create a Key-pair.

2. Edit Network settings. Select the VPC. Choose Subnet. Enable Auto-assign public IP.

3. Name the security group name accordingly and also for Description.

4. Launch Instance and Click Connect.

5. Switch to superuser and install nvm:

   ```bash
   sudo su -
   ```

   ```bash
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
   ```

6. Activate nvm: 

   ```bash
   . ~/.nvm/nvm.sh
   ```

7. Install the latest version of Node.js using nvm:

   ```bash
   nvm install node
   ```

8. Starting with Node.js 25, the Node team unbundled Corepack from the default installation. Now to install pnpm:

   ```bash
   npm install -g pnpm
   ```

9. And then we can check if the installation is successful:

   ```bash
   pnpm -v
   11.8.0
   ```

10. Install Git. Update the system and Install Git.

    ```bash
    sudo yum update -y
    ```

    ```bash
    sudo yum install git -y
    ```

    To check git version: 

    ```bash
    git --version
    ```

11. Then we can clone the github repo from the Github

    ```bash
    git clone [github-repo-link]
    ```

    Then we can press "ls" to check after completion and cd 

12. Next we navigate to the directory and install packages.

    ```bash
    cd [directory-name] 
    npm i or pnpm i
    ```

13. Create Env File and Port 80:

    ```bash
    echo "PORT=80" > .env
    ```

14. And then we can start the application.

    ```bash
    pnpm dev 
    ```

15. And then we can check if it is running by going back to the instance details in AWS Console and Copied the Public IPv4 address and open it in a browser. 

# Production Process Management with PM2

When deploying a Node.js/Go backend to an EC2 instance, running it directly in the terminal via `npm start`, `go run main.go`, or an executable binary is **not** production-ready. We use **PM2** (Process Manager 2) to solve these critical deployment challenges.

### 1. Keeps the Server Running in the Background (Daemonization)

- **The Problem:** If you start your server normally and close your SSH terminal connection (EC2 Instance Connect), the process dies instantly.
- **The PM2 Solution:** PM2 runs your application as a background daemon. You can safely disconnect from your EC2 instance, and your server will keep running on port 80.

### 2. Automatic Crash Recovery (Self-Healing)

- **The Problem:** If your code encounters an unhandled exception or runtime error, the process crashes and goes down completely, resulting in a `502 Bad Gateway` for your users.
- **The PM2 Solution:** PM2 constantly monitors the process. If it crashes, PM2 instantly restarts it in milliseconds, minimizing downtime.

### 3. Server Reboot Survival

- **The Problem:** If AWS reboots your EC2 instance for maintenance, or if it restarts due to a system issue, your application will not start back up automatically.
- **The PM2 Solution:** PM2 features a startup script generation tool. It hooks into the Linux system init system (`systemd`) to ensure your backend automatically launches the moment the EC2 instance powers on.

### 4. Zero-Downtime Reloads & Clustering

- **The Problem:** Updating your app usually requires killing the old process and starting a new one, causing a few seconds of downtime where users get errors.
- **The PM2 Solution:** PM2 can leverage multiple CPU cores (Cluster Mode) and allows you to run `pm2 reload`, which restarts instances one-by-one so your website never goes offline during a deployment.



---

# Install pm2 (Production Process Manager for Node.js)

1. Install pm2 globally:

   ```bash
   npm i pm2 -g
   ```

2. Modify the ecosystem file if necessary:

   ```bash
   nano ecosystem.config.js
   ```

3. Set pm2 to restart automatically on system reboot

   ```bash
   sudo env PATH=$PATH:$(which node) $(which pm2) startup systemd -u $USER --hp $(eval echo ~$USER)
   ```

4. Start the application using the pm2 ecosystem configuration:

   ```bash
   pm2 start ecosystem.config.js
   ```

5. Here we can run into a problem if we have defined the "type": "module" in our package.json in our project.

6. Troubleshooting: "ReferenceError: module is not defined in ES module scope".

   - **The Cause:** The project uses "type": "module" in package.json to enable modern ES6 imports (import express from 'express'). 
     - However, PM2 ecosystem files typically rely on classic Node Node.js module exports (module.exports = {}). Node gets confused trying to read CommonJS code inside an ES Module environment.

   - **The Fix:** Rename the file extension from .js to .cjs. The .cjs extension forces Node.js to interpret that specific file as standard CommonJS, bypassing the global package.json restriction.

7. Now we run this command in the terminal in EC2 Connect to rename it:

   ```bash
   mv ecosystem.config.js ecosystem.config.cjs
   ```

8. And then now we can try launching it again using the new filename:

   ```bash
   pm2 start ecosystem.config.cjs
   ```

9. After running successfully we can try running the following useful pm2 commands:

   ```bash
   pm2 monit # To monitor processes
   pm2 stop all # Stop all processes
   pm2 delete all # Delete all processes
   
   ```

   