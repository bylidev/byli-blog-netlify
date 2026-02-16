---
author: content/data/nacho.json
colors: bg-light-fg-dark
date: '2023-07-07'
featuredImage:
  altText: Managing Multiple Git Accounts with SSH Keys
  styles:
    self:
      borderRadius: medium
  type: ImageBlock
  url: /images/git.jpg
isFeatured: true
seo:
  metaDescription: Learn how to efficiently manage multiple Git accounts (personal and work) using SSH keys and configuration files. Complete guide with examples and troubleshooting tips.
  metaTitle: How to Use Multiple Git Accounts with SSH Keys | Complete Guide
  socialImage: /images/git.jpg
  type: Seo
slug: how-to-use-multiple-accounts-in-git
styles:
  self:
    flexDirection: col
title: How to Use Multiple Git Accounts with SSH Keys
type: PostLayout
---

## Introduction

Are you juggling between personal projects and work repositories? Switching between multiple Git accounts can be frustrating, especially when you accidentally commit with the wrong credentials. Fear not! This comprehensive guide will show you how to seamlessly manage multiple Git accounts using SSH keys and smart configuration.

**What you'll learn:**
- ✅ Generate and manage multiple SSH keys
- ✅ Configure Git to automatically use the right credentials
- ✅ Avoid authentication conflicts between accounts
- ✅ Troubleshoot common SSH-related issues

Let's dive in! 🚀

---

## Prerequisites

Before we begin, ensure you have:
- Git installed on your system
- Basic command-line knowledge
- Access to your Git hosting platforms (GitHub, GitLab, Bitbucket, etc.)
- A text editor (nano, vim, or VS Code)

---

## Step 1: Check Existing SSH Keys

First, let's see if you already have SSH keys on your system. Open your terminal and run:

```bash
ls -la ~/.ssh
```

**Common files you might see:**
- `id_rsa` / `id_rsa.pub` - RSA key pair
- `id_ed25519` / `id_ed25519.pub` - Ed25519 key pair (recommended)
- `known_hosts` - List of known SSH hosts
- `config` - SSH configuration file (we'll create this)

If you don't have any keys or need to create new ones, proceed to the next section.

---

## Step 2: Generate SSH Keys for Each Account

We'll create separate SSH keys for each Git account. The modern Ed25519 algorithm is recommended for better security and performance.

### For Personal Account

```bash
ssh-keygen -t ed25519 -C "personal@example.com" -f ~/.ssh/id_ed25519_personal
```

### For Work Account #1

```bash
ssh-keygen -t ed25519 -C "work1@company.com" -f ~/.ssh/id_ed25519_work1
```

### For Work Account #2

```bash
ssh-keygen -t ed25519 -C "work2@company.com" -f ~/.ssh/id_ed25519_work2
```

**💡 Pro Tips:**
- Use a strong passphrase for added security (optional but recommended)
- The `-f` flag specifies the filename to avoid overwriting existing keys
- Replace email addresses with your actual account emails

**Alternative (RSA keys):**
If your system doesn't support Ed25519, use RSA:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com" -f ~/.ssh/id_rsa_personal
```

---

## Step 3: Add SSH Keys to SSH Agent

Start the SSH agent and add your keys:

```bash
# Start the SSH agent
eval "$(ssh-agent -s)"

# Add your keys
ssh-add ~/.ssh/id_ed25519_personal
ssh-add ~/.ssh/id_ed25519_work1
ssh-add ~/.ssh/id_ed25519_work2
```

**Verify keys are added:**

```bash
ssh-add -l
```

You should see all your SSH keys listed with their fingerprints.

---

## Step 4: Add SSH Keys to Your Git Hosting Platforms

Copy each public key and add it to the corresponding Git account:

```bash
# Copy personal key
cat ~/.ssh/id_ed25519_personal.pub

# Copy work key 1
cat ~/.ssh/id_ed25519_work1.pub

# Copy work key 2
cat ~/.ssh/id_ed25519_work2.pub
```

**Then add them to your accounts:**
- **GitHub:** Settings → SSH and GPG keys → New SSH key
- **GitLab:** Preferences → SSH Keys → Add new key
- **Bitbucket:** Personal settings → SSH keys → Add key

---

## Step 5: Create SSH Config File

Now for the magic! Create a config file to automatically use the correct SSH key based on the hostname or directory.

```bash
nano ~/.ssh/config
```

### Configuration Option 1: Hostname-Based (Recommended)

This approach uses different host aliases for each account:

```ssh-config
# Personal GitHub Account
Host github.com-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_personal
  IdentitiesOnly yes

# Work GitHub Account
Host github.com-work1
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_work1
  IdentitiesOnly yes

# Work GitLab Account
Host gitlab.com-work2
  HostName gitlab.com
  User git
  IdentityFile ~/.ssh/id_ed25519_work2
  IdentitiesOnly yes

# Default GitHub (fallback)
Host github.com
  HostName ssh.github.com
  Port 443
  User git
  IdentityFile ~/.ssh/id_ed25519_personal
```

**Usage with this config:**

```bash
# Clone personal repo
git clone git@github.com-personal:username/personal-repo.git

# Clone work repo
git clone git@github.com-work1:company/work-repo.git
```

### Configuration Option 2: Directory-Based

This approach automatically selects the key based on your current working directory:

```ssh-config
# Default GitHub Configuration
Host github.com
  HostName ssh.github.com
  Port 443
  User git

# Match based on directory path
Match host github.com exec "pwd | grep -q personal"
  IdentityFile ~/.ssh/id_ed25519_personal

Match host github.com exec "pwd | grep -q work1"
  IdentityFile ~/.ssh/id_ed25519_work1

Match host gitlab.com exec "pwd | grep -q work2"
  IdentityFile ~/.ssh/id_ed25519_work2
```

**📝 Configuration Breakdown:**
- `Host` - Alias for the connection
- `HostName` - Actual hostname to connect to
- `User` - Username (usually "git" for Git hosting)
- `IdentityFile` - Path to the private SSH key
- `IdentitiesOnly yes` - Only use specified identity file
- `Match exec` - Execute command to determine if rule applies
- `Port 443` - Use HTTPS port (helps bypass firewall restrictions)

---

## Step 6: Configure Git User Settings Per Directory

For better organization, set up different Git configurations for each project directory:

### Global Fallback Configuration

```bash
git config --global user.name "Your Name"
git config --global user.email "personal@example.com"
```

### Directory-Specific Configuration

**For Personal Projects:**

```bash
cd ~/projects/personal
git config user.name "Your Name"
git config user.email "personal@example.com"
```

**For Work Projects:**

```bash
cd ~/projects/work1
git config user.name "Your Work Name"
git config user.email "work1@company.com"
```

### Using Conditional Includes (Advanced)

Edit your global Git config:

```bash
nano ~/.gitconfig
```

Add conditional includes:

```ini
[user]
    name = Your Name
    email = personal@example.com

[includeIf "gitdir:~/projects/work1/"]
    path = ~/.gitconfig-work1

[includeIf "gitdir:~/projects/work2/"]
    path = ~/.gitconfig-work2
```

Create separate config files:

```bash
# ~/.gitconfig-work1
[user]
    name = Your Work Name
    email = work1@company.com

# ~/.gitconfig-work2
[user]
    name = Your Other Work Name
    email = work2@company.com
```

---

## Step 7: Handle SSH Agent Persistence (Optional)

If you need to add SSH keys every time you open a terminal, add this to your shell configuration:

**For Bash (~/.bashrc):**

```bash
nano ~/.bashrc
```

Add the following:

```bash
# Start SSH agent and add keys automatically
if [ -z "$SSH_AUTH_SOCK" ]; then
  eval "$(ssh-agent -s)"
  ssh-add ~/.ssh/id_ed25519_personal
  ssh-add ~/.ssh/id_ed25519_work1
  ssh-add ~/.ssh/id_ed25519_work2
fi
```

**For Zsh (~/.zshrc):**

```bash
nano ~/.zshrc
```

Add the same configuration as above.

**Apply changes:**

```bash
source ~/.bashrc  # or source ~/.zshrc
```

---

## Step 8: Test Your Configuration

Let's verify everything is working correctly!

### Test SSH Connection

```bash
# Test personal account
ssh -T git@github.com-personal

# Test work account
ssh -T git@github.com-work1

# Test GitLab account
ssh -T git@gitlab.com-work2
```

**Expected output:**

```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

### Test with Real Repositories

```bash
# Navigate to personal project
cd ~/projects/personal/my-repo
git pull

# Check which identity is being used
ssh -vT git@github.com 2>&1 | grep "identity file"

# Navigate to work project
cd ~/projects/work1/company-repo
git pull

# Verify correct user is configured
git config user.email
```

### Test Commit with Correct Identity

```bash
# Make a test commit
echo "test" >> README.md
git add README.md
git commit -m "Test commit"
git log --format="%an <%ae>" -1
```

The output should show the correct name and email for that directory.

---

## Troubleshooting Common Issues

### Issue 1: Permission Denied (publickey)

**Problem:** `Permission denied (publickey)` error when trying to connect.

**Solutions:**

```bash
# Verify SSH key is added to agent
ssh-add -l

# Re-add the key
ssh-add ~/.ssh/id_ed25519_personal

# Test connection with verbose mode
ssh -vT git@github.com
```

### Issue 2: Wrong Identity Being Used

**Problem:** Commits are showing the wrong user.

**Solutions:**

```bash
# Check current Git config
git config user.email
git config user.name

# Override for specific repo
git config user.email "correct@example.com"
git config user.name "Correct Name"

# Amend last commit with correct author
git commit --amend --author="Name <email@example.com>" --no-edit
```

### Issue 3: SSH Agent Not Running

**Problem:** SSH keys not persisting between sessions.

**Solutions:**

```bash
# Start SSH agent
eval "$(ssh-agent -s)"

# Add persistent SSH agent script to shell config (see Step 7)
```

### Issue 4: Multiple Keys Causing Conflicts

**Problem:** SSH tries multiple keys and gets locked out.

**Solution:** Add `IdentitiesOnly yes` to your SSH config:

```ssh-config
Host github.com-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_personal
  IdentitiesOnly yes
```

### Issue 5: Firewall Blocking SSH Port 22

**Problem:** Cannot connect to Git hosting platform.

**Solution:** Use port 443 (HTTPS port):

```ssh-config
Host github.com
  HostName ssh.github.com
  Port 443
  User git
```

---

## Security Best Practices

🔒 **Keep Your Keys Secure:**
- Use strong passphrases for SSH keys
- Set proper permissions: `chmod 600 ~/.ssh/id_ed25519_*`
- Never share your private keys
- Keep private keys out of version control

🔐 **Regular Maintenance:**
- Rotate SSH keys periodically (every 6-12 months)
- Remove unused keys from Git hosting platforms
- Audit SSH keys: `ssh-add -l`

🛡️ **Additional Security:**
- Enable two-factor authentication (2FA) on Git accounts
- Use SSH key passphrases
- Consider using a hardware security key (YubiKey)

---

## Quick Reference Commands

```bash
# List SSH keys
ls -la ~/.ssh

# Generate new SSH key
ssh-keygen -t ed25519 -C "email@example.com" -f ~/.ssh/id_ed25519_name

# View public key
cat ~/.ssh/id_ed25519_name.pub

# Start SSH agent
eval "$(ssh-agent -s)"

# Add key to agent
ssh-add ~/.ssh/id_ed25519_name

# List added keys
ssh-add -l

# Remove all keys from agent
ssh-add -D

# Test SSH connection
ssh -T git@github.com

# Check Git config
git config --list

# Set local Git user
git config user.email "email@example.com"
git config user.name "Your Name"
```

---

## Conclusion

Congratulations! 🎉 You've successfully set up multiple Git accounts with SSH keys. Your workflow is now streamlined, and you'll never accidentally commit with the wrong account again.

**Key Takeaways:**
- ✅ Multiple SSH keys allow seamless account switching
- ✅ SSH config file automates key selection
- ✅ Directory-based or hostname-based approaches both work
- ✅ Git conditional includes provide additional flexibility
- ✅ Proper testing ensures everything works correctly

**Next Steps:**
- Set up GPG signing for commit verification
- Explore Git aliases for common operations
- Configure Git hooks for automated workflows
- Learn about Git credential helpers

Have questions or run into issues? Drop a comment below, and I'll be happy to help! 

Happy coding! 💻✨

---

## Related Articles

- [Understanding SSH Keys and Public Key Authentication](#)
- [Git Best Practices for Team Collaboration](#)
- [Advanced Git Configuration Tips](#)
- [How to Sign Git Commits with GPG](#)
