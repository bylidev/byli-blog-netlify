---
author: content/data/nacho.json
colors: bg-light-fg-dark
date: '2023-07-07'
featuredImage:
  altText: Thumbnail
  styles:
    self:
      borderRadius: medium
  type: ImageBlock
  url: /images/git.jpg
isFeatured: false
seo:
  metaDescription: How to use multiple accounts in git?.
  metaTitle: How to use multiple accounts in git?
  socialImage: /images/git.jpg
  type: Seo
slug: how-to-use-multiple-accounts-in-git
styles:
  self:
    flexDirection: col
title: How to use multiple accounts in git?
type: PostLayout
---

## How to Use Multiple Git Accounts with SSH Keys

### 1. Check Existing SSH Keys

Before proceeding, check if you already have SSH keys generated on your system. Open a terminal and run the following command:

`ls ~/.ssh` 

If you see files named `id_rsa` and `id_rsa.pub`, it means you already have an SSH key pair. If not, you can generate a new key pair using the following command:

`ssh-keygen -t rsa -C "your_email@example.com"` 

Replace `"your_email@example.com"` with your email address associated with the Git account. This will create a new SSH key pair in the `~/.ssh` directory.

### 2. Create a "config" File

Next, we'll create a `config` file in the `~/.ssh` directory to specify which SSH key to use for different Git accounts. Open the terminal and enter the following command:

`nano ~/.ssh/config` 

This will open the Nano text editor to create or edit the `config` file.

### 3. Add SSH Key Configuration

In the `config` file, we will define different configurations for each Git account based on specific conditions. For example:
```
Match host github.com exec "pwd | grep personal"
IdentityFile ~/.ssh/personal

Match host github.com exec "pwd | grep work_1"
IdentityFile ~/.ssh/work_1

Match host gitlab.com exec "pwd | grep work_2"
IdentityFile ~/.ssh/work_2
``` 

Explanation of the above configuration:

-   `Match`: This is used to match a specific condition before applying the configuration.
-   `host`: Specifies the hostname for which the configuration should be applied (e.g., github.com, gitlab.com).
-   `exec`: Executes a shell command to check if the condition is met. In this case, we use `pwd | grep` to match the current working directory.
-   `IdentityFile`: Specifies the path to the corresponding SSH private key for the matched condition.

The example assumes you have three SSH key pairs named `personal`, `work_1`, and `work_2`, and they correspond to different Git accounts on GitHub and GitLab.

### 4. Save and Exit

After adding the necessary configurations, save the file and exit the text editor. In Nano, you can do this by pressing `Ctrl + X`, then confirming the save by typing `Y` and pressing `Enter`.

### 5. Test the Configurations

To ensure that the configurations are working as expected, navigate to the directories associated with your different Git accounts and check if the corresponding SSH keys are being used. For example:

```
cd ~/projects/personal-repo
git pull

cd ~/projects/work-1-repo
git pull

cd ~/projects/work-2-repo
git pull
```

If everything is set up correctly, Git should automatically use the appropriate SSH key based on the directory you are in.

That's it! Now you can manage multiple Git accounts seamlessly with the right SSH keys, making your development workflow more efficient and hassle-free.