- **Set your username and email**:
```bash
git config --global user.name "Your Name" 
git config --global user.email "your_email@example.com"
```

- **Check your Git configuration** (view settings like your username and email):
```bash
git config --list
```

- **Set your default editor** (e.g., use VS Code for commit messages):
```bash
git config --global core.editor "code --wait"
```

- **Add ssh key**
```bash
`ssh-keygen -t rsa -b 4096 -C "your-new-email@example.com"
```

# Working with Repositories

- **Working with Repositories**:
```bash
git init
```

- **Add the Remote Repository**
```bash 
git remote add origin https:url-to-repo.git
```
make sure remote was added correctly:
```bash
git remote -v
```

- **Stage all modified files**:
```bash
git add .
```

- **Commit your changes with a message**:
```bash
git commit -m "Your commit message"
```

- **push your commit to branch:**
```bash
git branch -M main 
git push -u origin main
```
