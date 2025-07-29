# Git & SSH Key Setup on Arch Linux

This guide explains how to install `git`, configure your Git identity, and generate an SSH key for GitHub on Arch Linux using the username and email **`hypergolang`**.

---

## 1. Install Git and OpenSSH

First, ensure that `git` and `openssh` are installed on your Arch Linux system.

```bash
sudo pacman -Syu git openssh
````

---

## 2. Configure Git

Set your Git username and email (replace with your own if needed):

```bash
git config --global user.name "hypergolang"
git config --global user.email "hypergolang"
```

Check if the configuration is correct:

```bash
git config --list
```

---

## 3. Generate an SSH Key

Create a new **ed25519** SSH key (recommended for GitHub):

```bash
ssh-keygen -t ed25519 -C "hypergolang"
```

**When prompted:**

* `Enter a file in which to save the key`: Press **Enter** to use the default location (`~/.ssh/id_ed25519`).
* `Enter passphrase`: You can leave it empty by pressing **Enter** or set a password for extra security.

---

## 4. Start SSH Agent and Add Your Key

Start the SSH agent:

```bash
eval "$(ssh-agent -s)"
```

Add your private key:

```bash
ssh-add ~/.ssh/id_ed25519
```

---

## 5. Copy Your Public Key

Print your public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the entire output starting with `ssh-ed25519`.

---

## 6. Add SSH Key to GitHub

1. Go to [GitHub SSH Keys Settings](https://github.com/settings/keys).
2. Click **"New SSH Key"**.
3. Paste the copied key.
4. Add a descriptive title (e.g., `ArchLinux-PC`).
5. Save.

---

## 7. Test Your SSH Connection

Run:

```bash
ssh -T git@github.com
```

If successful, you'll see:

```
Hi hypergolang! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## 8. Auto-Load SSH Key on Startup

Create or edit the SSH config file:

```bash
nano ~/.ssh/config
```

Add:

```
Host github.com
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519
```

Save and exit.

---

## 9. Verify Everything Works

Clone a repo using SSH to test:

```bash
git clone git@github.com:your-username/your-repo.git
```

---

## Done!

You now have:

* Git configured with `hypergolang`.
* SSH keys generated and linked to GitHub.
* Auto SSH-agent setup.

---

```

---

### **Would you like me to create this `README.md` as a ready-to-download file and send it to you?**
```
