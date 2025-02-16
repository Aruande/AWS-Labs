# EC2 SSH Connection Troubleshooting Lab 🚀

## 📝 Overview
This lab demonstrates how to securely connect to an Amazon EC2 instance using SSH from a local machine.  
The instance runs **Amazon Linux 2023**, and authentication is done via an **SSH key pair**.

---

## 1⃣ Creating the EC2 Instance
1. **Logged into the AWS Console** → Navigated to **EC2**.
2. Clicked **Launch Instance** and selected **Amazon Linux 2023** as the OS.
3. **Created a new key pair** (`EC2Tutorial.pem`) and downloaded it.
4. **Configured Security Group**:
   - Allowed **SSH (port 22)** for inbound traffic from **my IP address only**.
5. Clicked **Launch** and waited for the instance to be ready.

---

## 2⃣ Configuring SSH on Local Machine
1. **Moved the private key (`.pem` file) to a secure directory**:
   ```bash
   mv ~/Downloads/EC2Tutorial.pem ~/Desktop/Aws_Solutions_Architect_Stuff/
   ```
2. **Set the correct permissions to prevent security issues**:
   ```bash
   chmod 400 ~/Desktop/Aws_Solutions_Architect_Stuff/EC2Tutorial.pem
   ```
3. **Verified that the key was in place**:
   ```bash
   ls -l ~/Desktop/Aws_Solutions_Architect_Stuff/EC2Tutorial.pem
   ```

---

## 3⃣ Attempting SSH Connection
1. **Connected to the EC2 instance using SSH**:
   ```bash
   ssh -i ~/Desktop/Aws_Solutions_Architect_Stuff/EC2Tutorial.pem ec2-user@<EC2-Public-IP>
   ```
2. **Received an error message**:
   ```bash
   Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
   ```

---

## 4⃣ Troubleshooting SSH Connection Issues

### ✅ **Step 1: Checked EC2 Instance Status**
- **Verified in the AWS Console** that the instance was **running**.

### ✅ **Step 2: Verified Security Group Settings**
- **Ensured port 22 (SSH) was open** to my **IP address**.

### ✅ **Step 3: Checked Key Permissions**
1. **Checked file permissions**:
   ```bash
   ls -l ~/Desktop/Aws_Solutions_Architect_Stuff/EC2Tutorial.pem
   ```
2. **If the permissions were too open, I reset them**:
   ```bash
   chmod 400 ~/Desktop/Aws_Solutions_Architect_Stuff/EC2Tutorial.pem
   ```

### ✅ **Step 4: Verified the Correct SSH User**
- **Amazon Linux instances use `ec2-user`, so I made sure I used**:
   ```bash
   ssh -i ~/Desktop/Aws_Solutions_Architect_Stuff/EC2Tutorial.pem ec2-user@<EC2-Public-IP>
   ```

### ✅ **Step 5: Checked the `~/.ssh/authorized_keys` File on the EC2 Instance**
1. **Connected using AWS Session Manager.**
2. **Checked the existing public keys in the instance**:
   ```bash
   cat ~/.ssh/authorized_keys
   ```
3. **Found that the wrong public key was listed.**

---

## 5⃣ Solution & Successful SSH Connection

### ✅ **The Fix**
1. **Copied the correct public key from my `.pem` file**:
   ```bash
   ssh-keygen -y -f ~/Desktop/Aws_Solutions_Architect_Stuff/EC2Tutorial.pem
   ```
2. **Updated the `~/.ssh/authorized_keys` file on the EC2 instance**:
   ```bash
   echo "<PASTE-PUBLIC-KEY-HERE>" >> ~/.ssh/authorized_keys
   chmod 600 ~/.ssh/authorized_keys
   ```

### 🎉 **Successfully Connected!**
- **Retried the SSH connection**:
   ```bash
   ssh -i ~/Desktop/Aws_Solutions_Architect_Stuff/EC2Tutorial.pem ec2-user@<EC2-Public-IP>
   ```
- **This time, I successfully accessed the EC2 instance!** 🎯

---

## 🔗 Key Takeaways
- **Always check that the correct key pair is being used.**
- **Ensure Security Groups allow SSH from your IP.**
- **Use the correct SSH user (`ec2-user` for Amazon Linux).**
- **If necessary, update `~/.ssh/authorized_keys` to include the right key.**

🚀 **This lab provided hands-on experience in troubleshooting SSH access to an EC2 instance.**

---

## 📝 Git Version Control Steps

### **1⃣ Add the Updated File to Git**
```bash
git add EC2_SSH_Lab.md
```

### **2⃣ Commit the Changes**
```bash
git commit -m "Updated EC2 SSH Lab documentation"
```

### **3⃣ Push the Changes to GitHub**
```bash
git push origin main
```

