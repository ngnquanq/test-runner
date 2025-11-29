# GitHub Actions Self-Hosted Runner Setup Guide

This guide will help you set up a self-hosted GitHub Actions runner on your local machine.

## Prerequisites

- A GitHub account and repository
- Linux machine (Ubuntu/Debian)
- Git installed
- Internet connection

## Step 1: Initialize Git Repository (if not already done)

```bash
cd /home/nhatquang/test-runner
git init
git add .
git commit -m "Initial commit with vulnerability scanning workflow"
```

## Step 2: Create GitHub Repository

1. Go to https://github.com/new
2. Create a new repository (e.g., "test-runner")
3. Don't initialize with README (we already have files)

## Step 3: Push Code to GitHub

```bash
git remote add origin https://github.com/YOUR_USERNAME/test-runner.git
git branch -M main
git push -u origin main
```

## Step 4: Set Up Self-Hosted Runner

1. Go to your repository on GitHub
2. Click **Settings** → **Actions** → **Runners**
3. Click **New self-hosted runner**
4. Select **Linux** as the OS

5. Follow the commands provided by GitHub (similar to below):

```bash
# Create a folder for the runner
mkdir -p ~/actions-runner && cd ~/actions-runner

# Download the latest runner package
curl -o actions-runner-linux-x64-2.311.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.311.0/actions-runner-linux-x64-2.311.0.tar.gz

# Extract the installer
tar xzf ./actions-runner-linux-x64-2.311.0.tar.gz

# Configure the runner
./config.sh --url https://github.com/YOUR_USERNAME/test-runner --token YOUR_TOKEN

# Run the runner
./run.sh
```

**Note:** GitHub will provide you with the exact commands including your token.

## Step 5: Start the Runner

After configuration, start the runner:

```bash
cd ~/actions-runner
./run.sh
```

You should see:
```
√ Connected to GitHub
√ Runner successfully registered
√ Listening for Jobs
```

## Step 6: Test the Pipeline

Now modify `requirements.txt` and push changes:

```bash
cd /home/nhatquang/test-runner

# Edit requirements.txt (add a new package)
echo "django==3.2.0" >> requirements.txt

# Commit and push
git add requirements.txt
git commit -m "Add django dependency"
git push
```

## Step 7: Watch the Magic!

1. Go to your repository on GitHub
2. Click **Actions** tab
3. You'll see the workflow running!
4. The runner on your local machine will execute the job
5. Check `vulnerability-reports/` folder for the scan results

## Running the Runner as a Service (Optional)

To keep the runner running in the background:

```bash
cd ~/actions-runner
sudo ./svc.sh install
sudo ./svc.sh start
sudo ./svc.sh status
```

## Troubleshooting

### Runner not picking up jobs
- Make sure the runner is online (check Settings → Actions → Runners)
- Verify the workflow uses `runs-on: self-hosted`

### Trivy installation fails
- The workflow will auto-install Trivy on first run
- If issues occur, manually install: `sudo apt-get update && sudo apt-get install trivy -y`

### Permission issues
- Ensure the runner user has write access to the repository directory
- Check `vulnerability-reports/` folder permissions

## What Happens in the Pipeline?

1. **Trigger**: Detects changes to `requirements.txt` when you push to GitHub
2. **Checkout**: Downloads the latest code to your self-hosted runner
3. **Install Trivy**: Installs the vulnerability scanner (if not present)
4. **Scan**: Analyzes `requirements.txt` for known vulnerabilities
5. **Report**: Saves results to `vulnerability-reports/` folder on your machine
6. **Display**: Shows the results in the GitHub Actions log

## Next Steps

- Try adding different packages to `requirements.txt`
- Check the vulnerability reports in the `vulnerability-reports/` folder
- Explore Trivy's different severity levels and formats
- Add more steps to the workflow (testing, building, etc.)

Happy CI/CD learning! 🚀
