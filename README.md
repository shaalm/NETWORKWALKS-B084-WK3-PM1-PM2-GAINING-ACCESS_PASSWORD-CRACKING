# 🔐 Networkwalks B084 - Week 3: Gaining Access & Password Cracking

Welcome to my Week 3 project repository for the **Cybersecurity & Ethical Hacking** course by Networkwalks. 

This repository contains the hands-on labs, documentation, and video demonstrations for **Password Cracking**. The labs explore traditional methods using John the Ripper and Networkwalks custom tools, as well as cutting-edge **AI-assisted penetration testing** using the Hexstrike MCP Server and Claude Desktop.

## 📂 Modules Overview

*   **[Project Module 1: Password Cracking with JTR](W3-PM1%20-%20Week3%20-%20Project%20Module1%20-%20Password%20Cracking%20with%20JTR%20v1.pdf)**
*   **[Project Module 2: Password Cracking with NW Tools](W3-PM2%20-%20Week3%20-%20Project%20Module2%20-%20Password%20Cracking%20with%20NW%20Tools%20v1.pdf)**
*   **[Optional Module: AI-Assisted JTR Password Cracking](z.%20Optional%20Module%20Lab%20-%20JTR%20Password%20Cracking%20Lab%20v1%20(AI-version).pdf)**
*   **[Hexstrike MCP Setup Guide](7.2.1.%20LAB%20PRACTICE%20-%20How%20to%20setup%20Hexstrike%20MCP%20with%20Claude%20v1.pdf)**

---

## 🎥 Video Demonstrations
**Watch the experiments in action!** 

*   📺 **[Video 1: Cracking PDF passwords using JTR & Johnny on Windows](https://www.youtube.com/watch?v=I1KkenC71pU)**
*   📺 **[Video 2: Cracking PDF passwords using Networkwalks Web Tools](https://www.youtube.com/watch?v=2IQ5poevRng)**
*   📺 **[Video 3: AI-Assisted Cracking using Claude Desktop & Hexstrike MCP](https://www.youtube.com/watch?v=TO6e2O9lR5k)**
*   📺 **[Video 4: AI-Assisted Cracking using Claude Desktop & Hexstrike MCP (Part 2)](https://www.youtube.com/watch?v=54h7yoRTPWk)**

---

## 🛠️ Lab 1 & 2: Traditional Password Cracking
**Objective:** Recover the password from the locked `My Locked PDF1.pdf` file.

### Part 1: John the Ripper (JTR) & Johnny GUI
We installed John the Ripper and its graphical interface, Johnny, on a Windows environment. 
1. Extracted the `$pdf$...` hash from the target PDF.
2. Saved the hash into a `.txt` file.
3. Used Johnny to initiate a dictionary attack, successfully recovering the password.

### Part 2: Networkwalks Web Tools
To show a browser-based approach, we utilized the **Networkwalks Hash Calculator** and **Password Cracker**.
1. Uploaded the locked PDF directly to the Hash Calculator to extract the hash.
2. Input the hash into the Password Cracker tool.
3. Successfully cracked the password (`password1`).

---

## 🤖 Optional Lab: AI-Assisted Cracking with Hexstrike & Claude
**Objective:** Setup the Hexstrike-AI MCP (Model Context Protocol) Server with Claude Desktop on Kali Linux, and instruct the AI to autonomously crack a PDF password using JTR and the `rockyou.txt` wordlist.

### 🛠️ Hexstrike MCP Setup on Kali Linux
We installed Claude Desktop on Kali Linux using the Debian repository, then cloned and configured the Hexstrike-AI server.

**1. Install Claude Desktop:**
```bash
# Add the GPG key
curl -fsSL https://pkg.claude-desktop-debian.dev/KEY.gpg | sudo gpg --dearmor -o /usr/share/keyrings/claude-desktop.gpg

# Add the repository
echo "deb [signed-by=/usr/share/keyrings/claude-desktop.gpg arch=amd64,arm64] https://pkg.claude-desktop-debian.dev stable main" | sudo tee /etc/apt/sources.list.d/claude-desktop.list

# Update and install
sudo apt update
sudo apt install claude-desktop
```

**2. Setup Hexstrike-AI:**
```bash
# Clone the repository
git clone https://github.com/0x4m4/hexstrike-ai.git
cd hexstrike-ai

# Create virtual environment
python3 -m venv hexstrike-env
source hexstrike-env/bin/activate

# Install dependencies
pip3 install -r requirements.txt

# Start the server
python3 hexstrike_server.py
```

**3. Configure Claude Desktop MCP:**
Added the following configuration to the `claude_desktop_config.json` file:

```json
{
  "mcpServers": {
    "hexstrike-ai": {
      "command": "/home/kali/hexstrike-ai/hexstrike-env/bin/python",
      "args": [
        "/home/kali/hexstrike-ai/hexstrike_mcp.py",
        "--server",
        "http://localhost:8888"
      ]
    }
  }
}
```

### 🧠 The AI Prompts Used
Once the server was running and connected to Claude, the following prompts were used to crack the target file (`networkwalks_flag1.pdf`):

1. *"Check if John the Ripper is installed in this Hexstrike MCP and show me its version."*
2. *"Please calculate the hash value of this PDF file: `/home/kali/Desktop/hash3.networkwalks_flag1.pdf`"*
3. *"Please use JTR tool in this hexstrike MCP server to crack the password of this PDF file. Use the rockyou.txt wordlist dictionary."*

**Result:** Claude AI autonomously executed the commands, utilized JTR, and successfully cracked the password (`password1`) using the `rockyou.txt` wordlist!

---

## 💡 Key Takeaways
*   **Encryption vs. Hashing:** While encryption is a two-way function (used for confidentiality), hashing is a one-way function (used for integrity/validation). PDF passwords are stored as hashes, which is why tools like JTR can crack them by comparing generated hashes to the stored one.
*   **Password Strength:** Weak or common passwords (like `password1`) can be cracked in seconds using standard wordlists.
*   **The Power of AI:** Integrating LLMs (like Claude) with MCP servers (like Hexstrike) transforms AI from a simple chatbot into a powerful, autonomous cybersecurity assistant capable of executing complex penetration testing tools via natural language.

## 🧰 Tools Used
*   **John the Ripper / Johnny** (Windows)
*   **Networkwalks Hash Calculator & Password Cracker** (Web Browser)
*   **Kali Linux** (VM)
*   **Claude Desktop**
*   **Hexstrike-AI MCP Server**

---
*Disclaimer: All activities in this repository were performed in a controlled lab environment for educational purposes only. Never attempt to crack passwords on systems you do not own or have explicit permission to test.*
```
