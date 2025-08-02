# Git & GitHub Setup on Arch Linux

This guide explains how to install `git`, configure your Git identity, and set up GitHub authentication on Arch Linux using the username and email **`hypergolang`**.

There are **two methods** to authenticate with GitHub: **SSH Keys** (traditional) and **GitHub CLI** (modern, easier for VS Code integration).

---

## Method 1: SSH Key Setup (Traditional)

### 1. Install Git and OpenSSH
```bash
sudo pacman -Syu git openssh
```

### 2. Configure Git
Set your Git username and email:
```bash
git config --global user.name "hypergolang"
git config --global user.email "hypergolang@gmail.com"
```

Check configuration:
```bash
git config --list
```

### 3. Generate SSH Key
Create a new **ed25519** SSH key:
```bash
ssh-keygen -t ed25519 -C "hypergolang"
```

**When prompted:**
* *Enter file location*: Press **Enter** for default (`~/.ssh/id_ed25519`)
* *Enter passphrase*: Press **Enter** for no password, or set one for security

### 4. Start SSH Agent and Add Key
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### 5. Copy Public Key
```bash
cat ~/.ssh/id_ed25519.pub
```
Copy the entire output starting with `ssh-ed25519`.

### 6. Add SSH Key to GitHub
1. Go to [GitHub SSH Keys Settings](https://github.com/settings/keys)
2. Click **"New SSH Key"**
3. Paste the copied key
4. Add title (e.g., `ArchLinux-PC`)
5. Save

### 7. Test SSH Connection
```bash
ssh -T git@github.com
```

Expected output:
```
Hi hypergolang! You've successfully authenticated, but GitHub does not provide shell access.
```

### 8. Auto-Load SSH Key on Startup
Create SSH config:
```bash
vim ~/.ssh/config
```

Add:
```
Host github.com
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519
```

---

## Method 2: GitHub CLI Setup (Recommended for VS Code)

### 1. Install GitHub CLI
```bash
sudo pacman -S github-cli
```

### 2. Install Git (if not already installed)
```bash
sudo pacman -S git
```

### 3. Configure Git
```bash
git config --global user.name "hypergolang"
git config --global user.email "hypergolang@gmail.com"
```

### 4. Login to GitHub CLI
```bash
gh auth login
```

**Choose these options:**
* **Where do you use GitHub?** → `GitHub.com`
* **Preferred protocol?** → `HTTPS`
* **Authenticate Git with GitHub credentials?** → `Yes`
* **How to authenticate?** → `Login with a web browser`

Follow the browser authentication process.

### 5. Verify Authentication
```bash
gh auth status
```

You should see:
```
✓ Logged in to github.com as hypergolang
```

### 6. Test Git Operations
```bash
git config --global credential.helper ""
git config --global credential.helper manager
```

---

## Which Method to Choose?

### Use **SSH Keys** if:
- You prefer traditional Git workflow
- You work primarily in terminal
- You want to avoid browser authentication

### Use **GitHub CLI** if:
- You use VS Code with GitHub extensions
- You want seamless integration with GitHub features (PRs, Issues)
- You prefer HTTPS over SSH
- You want to avoid VS Code authentication popups

---

## Testing Your Setup

### For SSH Method:
```bash
git clone git@github.com:username/your-repo.git
```

### For GitHub CLI Method:
```bash
git clone https://github.com/username/your-repo.git
```

---

## VS Code Integration

If using **GitHub CLI method**, VS Code will automatically use your authenticated GitHub CLI session without additional popups.

If using **SSH method**, you may need to configure VS Code's Git settings to use SSH.

---

## Troubleshooting

### SSH Issues:
- Make sure SSH agent is running: `ssh-add -l`
- Test connection: `ssh -T git@github.com`

### GitHub CLI Issues:
- Check auth status: `gh auth status`
- Re-login if needed: `gh auth login`

### Git Push Issues:
- For SSH: Use `git@github.com:username/repo.git` format
- For HTTPS: Use `https://github.com/username/repo.git` format

---

## Done!

You now have Git configured with GitHub authentication using your preferred method. Both methods will work seamlessly with your development workflow.
