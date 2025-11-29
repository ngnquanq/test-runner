# CI/CD Learning Project: GitHub Actions Self-Hosted Runner

This project demonstrates a simple CI/CD pipeline using GitHub Actions with a self-hosted runner.

## What This Project Does

When you push changes to `requirements.txt` on GitHub, it automatically:
1. Detects the changes
2. Triggers the workflow on your local machine (self-hosted runner)
3. Runs Trivy vulnerability scanner
4. Saves the security report to `vulnerability-reports/` folder on your machine

## Project Structure

```
test-runner/
├── .github/
│   └── workflows/
│       └── vulnerability-scan.yml    # GitHub Actions workflow
├── requirements.txt                  # Python dependencies to scan
├── vulnerability-reports/            # Scan results stored here
├── SETUP_RUNNER.md                   # Detailed setup instructions
└── README.md                         # This file
```

## Quick Start

1. **Set up the repository on GitHub:**
   - Follow the instructions in `SETUP_RUNNER.md`

2. **Configure the self-hosted runner:**
   - Go to your GitHub repo Settings → Actions → Runners
   - Add a new self-hosted runner
   - Run the setup commands on your machine

3. **Test the pipeline:**
   - Modify `requirements.txt`
   - Push to GitHub
   - Watch the workflow run on your local machine!

## Example: Testing the Pipeline

```bash
# Add a new dependency
echo "pandas==1.5.0" >> requirements.txt

# Commit and push
git add requirements.txt
git commit -m "Add pandas dependency"
git push

# Check the Actions tab on GitHub to see it running
# Check vulnerability-reports/ folder for the results
```

## What You'll Learn

- How GitHub Actions self-hosted runners work
- How to trigger workflows on specific file changes
- How to run security scans in CI/CD
- How to save artifacts locally from automated workflows

## Files Explained

### `.github/workflows/vulnerability-scan.yml`
The GitHub Actions workflow that:
- Triggers on changes to `requirements.txt`
- Runs on your self-hosted runner
- Installs and runs Trivy scanner
- Saves reports locally

### `requirements.txt`
Example Python dependencies to scan for vulnerabilities.

### `vulnerability-reports/`
Directory where scan results are saved (timestamped).

## For More Details

See `SETUP_RUNNER.md` for complete step-by-step setup instructions.
