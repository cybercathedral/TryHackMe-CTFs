# 🏰 CyberCathedral: Genesis of the Cipher - Beginner's Walkthrough

![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-brightgreen)
![Category](https://img.shields.io/badge/Category-Steganography-blue)
![Skills](https://img.shields.io/badge/Skills-Linux%20%7C%20Forensics-orange)

A step-by-step guide for solving the CyberCathedral Genesis CTF challenge. Perfect for beginners learning steganography and digital forensics!

---

## 📋 Table of Contents

- [What You'll Learn](#-what-youll-learn)
- [Prerequisites](#-prerequisites)
- [Getting Started](#-getting-started)
- [Solution Steps](#-solution-steps)
- [Complete Flag](#-complete-flag)
- [Troubleshooting](#-troubleshooting)
- [Key Concepts Explained](#-key-concepts-explained)
- [Additional Resources](#-additional-resources)

---

## 🎯 What You'll Learn

By completing this challenge, you'll gain hands-on experience with:

- **Steganography** - Hiding and extracting secret data from images
- **Linux Command Line** - Essential terminal commands
- **Base64 Encoding** - A common data encoding method
- **File System Navigation** - Finding hidden files and directories
- **CTF Methodology** - Logical problem-solving approaches

---

## 🛠️ Prerequisites

### Required Tools

Before starting, make sure you have these tools installed:

#### On Linux (Ubuntu/Debian):
```bash
sudo apt-get update
sudo apt-get install steghide
```

#### On macOS:
```bash
brew install steghide
```

#### On Windows:
- Install [WSL (Windows Subsystem for Linux)](https://docs.microsoft.com/en-us/windows/wsl/install)
- Then follow Linux installation steps

### Basic Knowledge Needed

- How to open a terminal/command prompt
- Basic file navigation (`cd`, `ls`)
- How to read text files (`cat`)

**Don't worry if you're new to these!** We'll explain each command as we go.

---

## 🚀 Getting Started

### Step 0: Download and Extract the Challenge

1. Download the challenge file: `CyberCathedral_Genesis_CTF.zip`
2. Extract it to a folder
3. Open your terminal and navigate to the extracted folder:

```bash
cd path/to/CyberCathedral_Genesis_CTF
```

**What does this do?** The `cd` command means "change directory" - it moves you into the challenge folder.

### Explore What's Inside

```bash
ls -la
```

**What does this do?** 
- `ls` = lists files in the current directory
- `-la` = shows ALL files, including hidden ones (files starting with `.`)

You should see several files including:
- `PROJECT_BRIEF.md`
- `system_config.ini`
- Several `.jpg` image files
- Possibly some hidden directories (starting with `.`)

---

## 🔍 Solution Steps

### 🔑 Step 1: Extract from the First Image

Let's start with `system_blueprint.jpg`. We'll use a tool called **steghide** to extract hidden data.

```bash
steghide extract -sf system_blueprint.jpg -p "cathedral"
```

**Breaking down this command:**
- `steghide extract` = extract hidden data
- `-sf system_blueprint.jpg` = from this specific file
- `-p "cathedral"` = using this password

**Output:** A file called `system_note.txt` will be created.

Now read what's inside:

```bash
cat system_note.txt
```

**Result:** You'll see: `System authentication requires passphrase: cathedral`

✅ **What we learned:** The password "cathedral" works! This confirms we're on the right track.

---

### 🔑 Step 2: Move to the Second Image

Now try the next image with the same password:

```bash
steghide extract -sf architecture_diagram.jpg -p "cathedral"
```

This creates a file called `module_key.enc`. Let's read it:

```bash
cat module_key.enc
```

You'll see something that looks scrambled. This is **Base64 encoded** data. Let's decode it:

```bash
cat module_key.enc | base64 -d
```

**Breaking down this command:**
- `cat module_key.enc` = read the file
- `|` = pipe (send the output to the next command)
- `base64 -d` = decode from Base64 format

**Result:** `Next authentication token: shadow`

✅ **What we learned:** Our next password is "shadow", and we now know about Base64 encoding!

---

### 🔑 Step 3: Third Image with New Password

Use our newly discovered password on the next image:

```bash
steghide extract -sf security_protocol.jpg -p "shadow"
```

Read the extracted file:

```bash
cat security_directive.txt
```

**Result:** `Final backup location: .system_backup/archives. Check config for default passphrase.`

✅ **What we learned:** 
1. There's a hidden directory called `.system_backup/archives`
2. We need to check the config file for a password

---

### 🔑 Step 4: Find the Hidden Directory

First, let's check that configuration file:

```bash
cat system_config.ini
```

Look for a line that says: `default_passphrase=genesis`

✅ **Found it!** Our final password is "genesis"

Now navigate to the hidden backup directory:

```bash
cd .system_backup/archives
```

**Note:** The dot (`.`) at the beginning makes this a hidden directory, which is why we needed `ls -la` to see it!

List what's inside:

```bash
ls -la
```

You should see `foundation_backup.jpg`

---

### 🔑 Step 5: Extract the Final Flag

Use the password from the config file:

```bash
steghide extract -sf foundation_backup.jpg -p "genesis"
```

Read the final file:

```bash
cat true_genesis.txt
```

---

## 🏆 Complete Flag

```
CYC{G3n3s1s_0f_Th3_C1ph3r_Unv31l3d}
```

**Congratulations!** 🎉 You've successfully completed the challenge!

---

## 🔧 Troubleshooting

### Problem: "steghide: command not found"

**Solution:** You need to install steghide first. See the [Prerequisites](#-prerequisites) section.

### Problem: "could not extract any data"

**Solution:** 
- Double-check you're using the correct password
- Make sure you're using the right image file
- Verify the image file isn't corrupted

### Problem: "No such file or directory"

**Solution:**
- Use `pwd` to check your current directory
- Use `ls` to see what files are available
- Make sure you've extracted all files from the ZIP

### Problem: Can't see hidden directories

**Solution:** Always use `ls -la` instead of just `ls` to show hidden files and directories.

---

## 📚 Key Concepts Explained

### What is Steganography?

Steganography is the practice of hiding secret data inside ordinary files (like images). Unlike encryption, which scrambles data, steganography hides the very existence of the data.

**Real-world example:** Imagine writing a secret message in invisible ink between the lines of a normal letter.

### What is Base64?

Base64 is a way to encode binary data as text. It's commonly used to transmit data over systems that only support text.

**Example:** The text "Hello" in Base64 becomes "SGVsbG8="

### Hidden Files in Linux

In Linux and Unix systems, any file or directory starting with a dot (`.`) is hidden by default. Examples:
- `.system_backup` - hidden directory
- `.bashrc` - hidden configuration file

### The CTF Methodology

Most CTF challenges follow a pattern:
1. **Reconnaissance** - Explore and gather information
2. **Analysis** - Examine what you found
3. **Exploitation** - Use what you learned to progress
4. **Capture** - Retrieve the flag

This challenge perfectly demonstrates this methodology!

---

## 🎓 Additional Resources

### Want to Learn More?

- **TryHackMe** - Interactive cybersecurity learning platform
- **OverTheWire: Bandit** - Learn Linux command line through games
- **CyberChef** - Web tool for encoding/decoding data
- **CTFtime** - Calendar of upcoming CTF competitions

### Practice Similar Challenges

- **PicoCTF** - Beginner-friendly CTF platform
- **HackTheBox** - More advanced security challenges
- **SANS Holiday Hack Challenge** - Annual beginner-friendly CTF

### Join the Community

- r/cybersecurity - Reddit community
- Discord servers for CTF discussions
- Local cybersecurity meetups and conferences

---

## 📝 Quick Reference

### Commands Used in This Challenge

| Command | Purpose | Example |
|---------|---------|---------|
| `cd` | Change directory | `cd .system_backup/archives` |
| `ls -la` | List all files (including hidden) | `ls -la` |
| `cat` | Display file contents | `cat file.txt` |
| `steghide extract` | Extract hidden data from image | `steghide extract -sf image.jpg -p password` |
| `base64 -d` | Decode Base64 data | `cat file | base64 -d` |
| `pwd` | Print working directory | `pwd` |

### Passwords Used

1. First extraction: `cathedral`
2. Second extraction: `cathedral`
3. Third extraction: `shadow`
4. Final extraction: `genesis`

---

## 💡 Pro Tips for Future CTFs

✨ **Always read documentation files** - Files like README, config files, and notes often contain crucial clues

✨ **Take notes** - Keep track of passwords, file locations, and discoveries

✨ **Try the simple things first** - Don't overcomplicate; the obvious answer is often correct

✨ **Search for patterns** - CTF flags usually follow a format like `FLAG{...}` or `CYC{...}`

✨ **Use `file` command** - Type `file filename` to identify file types

✨ **Google is your friend** - Don't know a command? Look it up!

---

## 🤝 Contributing

Found a mistake or want to improve this walkthrough? Feel free to submit a pull request or open an issue!

---

## 📄 License

This walkthrough is provided for educational purposes. Please use these techniques responsibly and only on systems you have permission to test.

---

**Happy Hacking!** 🔐

*Remember: The goal of CTFs is to learn. Don't get discouraged if you get stuck - every expert was once a beginner!*
