# AI Models in Smart Resume Analyzer

Smart Resume Analyzer uses advanced AI models to provide detailed analysis and feedback on your resume. This document explains the AI models integrated into the application and how they work.

## Available AI Models

### 1. Google Gemini

Google Gemini is a powerful AI model developed by Google that offers state-of-the-art natural language processing capabilities. In Smart Resume Analyzer, Gemini is used to:

- Analyze resume content and structure
- Identify key skills and missing skills for target roles
- Provide personalized recommendations for improvement
- Score resumes based on quality and relevance

## How AI Analysis Works

When you upload your resume for AI analysis, the following process occurs:

1. **Text Extraction**: The system extracts text from your PDF or DOCX resume
2. **AI Processing**: The selected AI model (Gemini or Claude) analyzes the resume text
3. **Structured Analysis**: The AI generates a structured analysis including:
   - Overall assessment
   - Skills analysis (current and missing skills)
   - Strengths
   - Areas for improvement
   - Recommended courses
   - Resume score (0-100)
  
# Deployment Guide for Smart AI Resume Analyzer

This guide provides instructions for deploying the Smart AI Resume Analyzer application in various environments, with a focus on resolving Chrome webdriver issues.

## Recommended CI/CD: GitHub Pages + Streamlit Cloud

This repository uses a two-part deployment model:

- GitHub Pages hosts a static landing page from `typro/docs`
- Streamlit Community Cloud hosts the live Python application

Why this split is required:

- GitHub Pages can only serve static files
- This project needs a running Python process (`streamlit run app.py`), so it must run on a Python host

### GitHub Actions Workflows

At repository root (`C:\TYY`):

- `.github/workflows/ci.yml`
   - Runs on push/PR to `main`
   - Installs `typro/requirements.txt`
   - Compiles Python files for syntax validation

- `.github/workflows/pages.yml`
   - Runs on push to `main` when `typro/docs/**` changes
   - Publishes `typro/docs` to GitHub Pages

### One-time GitHub Repository Settings

1. Go to **Settings → Pages**
2. Under **Build and deployment**, choose **GitHub Actions**
3. Ensure your default branch is `main`

### Streamlit Cloud Setup

1. Open Streamlit Community Cloud and create a new app from your GitHub repo
2. Set **Main file path** to `typro/app.py` (or `typro/run_app.py` if preferred)
3. Add required secrets in Streamlit **Secrets** UI (do not commit secrets in repo)
4. Keep `typro/packages.txt` and `typro/requirements.txt` in sync with runtime needs

### Update the GitHub Pages App Link

After Streamlit deploys, edit:

- `typro/docs/index.html`

Replace the placeholder `https://share.streamlit.io/` link with your actual Streamlit app URL.

## Local Deployment

### Prerequisites
- Python 3.7 or higher
- Chrome browser installed
- pip for installing dependencies

### Steps for Windows
1. Clone the repository
2. Install dependencies:
   ```
   pip install -r requirements.txt
   ```
3. Run the application using the Python script:
   ```
   python run_app.py
   ```
   
   This script will automatically set up chromedriver and start the application.

   Alternatively, you can run the batch file:
   ```
   startup.bat
   ```

### Steps for Linux/Mac
1. Clone the repository
2. Install dependencies:
   ```
   pip install -r requirements.txt
   ```
3. Run the setup script to install the correct chromedriver:
   ```
   python setup_chromedriver.py
   ```
4. Run the application:
   ```
   streamlit run app.py
   ```

   Alternatively, you can use the startup script which handles both chromedriver setup and application startup:
   ```
   chmod +x startup.sh
   ./startup.sh
   ```

## Server Deployment (Linux)

### Installing Chrome on Ubuntu/Debian
```bash
# Update package list
sudo apt update

# Install Chrome dependencies
sudo apt install -y wget unzip fontconfig fonts-liberation libasound2 libatk-bridge2.0-0 libatk1.0-0 libatspi2.0-0 libcairo2 libcups2 libdrm2 libgbm1 libgtk-3-0 libnspr4 libnss3 libpango-1.0-0 libxcomposite1 libxdamage1 libxfixes3 libxkbcommon0 libxrandr2 xdg-utils

# Download and install Chrome
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install -y ./google-chrome-stable_current_amd64.deb
rm google-chrome-stable_current_amd64.deb

# Verify installation
google-chrome --version
```

### Installing Chrome on CentOS/RHEL
```bash
# Add Chrome repository
sudo tee /etc/yum.repos.d/google-chrome.repo <<EOF
[google-chrome]
name=google-chrome
baseurl=http://dl.google.com/linux/chrome/rpm/stable/x86_64
enabled=1
gpgcheck=1
gpgkey=https://dl.google.com/linux/linux_signing_key.pub
EOF

# Install Chrome
sudo yum install -y google-chrome-stable

# Verify installation
google-chrome --version
```

### Running the Application on Server
After installing Chrome, deploy the application:

1. Clone the repository
2. Install dependencies:
   ```
   pip install -r requirements.txt
   ```
3. Make the startup script executable and run it:
   ```
   chmod +x startup.sh
   ./startup.sh
   ```

## Windows Server Deployment

### Prerequisites
- Python 3.7 or higher
- Chrome browser installed
- pip for installing dependencies

### Steps
1. Clone the repository
2. Install dependencies:
   ```
   pip install -r requirements.txt
   ```
3. Run the application using the Python script:
   ```
   python run_app.py
   ```
   
   This script will automatically set up chromedriver and start the application.

## Streamlit Cloud Deployment

When deploying to Streamlit Cloud, you need to ensure Chrome is available. Our application includes multiple fallback mechanisms to handle this.

### Steps for Streamlit Cloud
1. Push your code to a GitHub repository
2. Create a new app in Streamlit Cloud pointing to your repository
3. Make sure your `requirements.txt` includes all necessary dependencies:
   - `selenium>=4.10.0`
   - `webdriver-manager>=4.0.0`
   - `chromedriver-autoinstaller>=0.6.2`
4. Ensure the `packages.txt` file is in your repository with:
   ```
   chromium
   chromium-driver
   libglib2.0-0
   libnss3
   libgconf-2-4
   libfontconfig1
   xvfb
   wget
   unzip
   ```

### Troubleshooting Streamlit Cloud
If you encounter issues with Chrome on Streamlit Cloud:

1. Check the logs for specific error messages
2. Try adding a custom command to run the setup script before the app starts:
   - In the Streamlit Cloud settings, add a "Main file path" of `run_app.py` instead of `app.py`

## Docker Deployment

For Docker deployment, you need to include Chrome in your Docker image.

### Sample Dockerfile
```dockerfile
FROM python:3.9-slim

# Install Chrome
RUN apt-get update && apt-get install -y \
    wget \
    gnupg \
    unzip \
    xvfb \
    libglib2.0-0 \
    libnss3 \
    libgconf-2-4 \
    libfontconfig1 \
    && wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list \
    && apt-get update \
    && apt-get install -y google-chrome-stable \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Set up working directory
WORKDIR /app

# Copy requirements and install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Make scripts executable
RUN chmod +x startup.sh setup_chromedriver.py

# Expose port for Streamlit
EXPOSE 8501

# Run the startup script
CMD ["python", "run_app.py"]
```

## Troubleshooting Common Issues

### Error: "Service unexpectedly exited"
This usually indicates that Chrome cannot be started. Ensure:
- Chrome is installed
- You have the correct permissions
- You're using the `--no-sandbox` option in headless environments

### Error: "Chrome version must be between X and Y"
This indicates a version mismatch between Chrome and chromedriver:
- Run the `setup_chromedriver.py` script to install the matching chromedriver version
- The script automatically detects your Chrome version and downloads the compatible chromedriver

### Error: "Permission denied" when installing chromedriver
This is a permission issue:
- Try running the application with administrator privileges
- Ensure the user has write permissions to the installation directory
- Use the `setup_chromedriver.py` script which installs chromedriver in the user's home directory

### Error: "unknown error: DevToolsActivePort file doesn't exist"
This is common in containerized environments:
- Add `--disable-dev-shm-usage` to Chrome options (already included in our setup)
- Ensure you're using `--no-sandbox` in Docker/container environments

### Windows-specific issues
If you encounter issues on Windows:
- Make sure Chrome is installed in the standard location
- Try running the application as administrator
- Use the `run_app.py` script which handles setup automatically
- Check Windows Defender or antivirus software that might be blocking chromedriver

## Additional Resources
- [Selenium Documentation](https://www.selenium.dev/documentation/)
- [Chrome for Testing](https://developer.chrome.com/docs/chromium/chrome-for-testing)
- [Streamlit Deployment](https://docs.streamlit.io/streamlit-cloud/get-started/deploy-an-app) 

## Configuring AI Models

To use these AI models, you need to set up API keys in your `.env` file:

```
# API Keys for AI Models
GOOGLE_API_KEY=your_google_api_key_here
```

- For Google Gemini, you need a Google API key from [Google AI Studio](https://makersuite.google.com/)

## Privacy and Data Handling

When using the AI analysis features:

- Resume data is sent to the respective AI model providers (Google or Anthropic via OpenRouter)
- Analysis results are stored in the local database for reference
- No personal data is shared with third parties beyond what's necessary for analysis
- You can delete your data at any time through the application

## Future AI Integrations

We plan to integrate additional AI models in the future to provide even more comprehensive resume analysis and feedback. Stay tuned for updates! 

# Hybird Application System - Run Guide

This file contains the exact steps to run the app locally on Windows.

## 1) Open project folder

Open terminal and run:

```powershell
cd C:\TYY\typro
```

## 2) Activate Python virtual environment

If your environment is at `C:\TYY\.venv`:

```powershell
C:\TYY\.venv\Scripts\activate
```

If your environment is inside project as `venv`:

```powershell
c
```

## 3) Install dependencies (first time only)

```powershell
pip install -r requirements.txt
```

## 4) Run the Streamlit app

```powershell
streamlit run app.py
```

Alternative (explicit Python path):

```powershell
C:\TYY\.venv\Scripts\python.exe -m streamlit run app.py
```

## 5) Open in browser

Use:

- http://localhost:8501

## 6) If port 8501 is busy

```powershell
streamlit run app.py --server.port 8502
```

Then open:

- http://localhost:8502

## 7) Stop the app

In terminal where Streamlit is running, press:

- `Ctrl + C`

## Quick health check (optional)

```powershell
try { (Invoke-WebRequest -Uri http://localhost:8501 -UseBasicParsing -TimeoutSec 10).StatusCode } catch { $_.Exception.Message }
```

Expected output when running correctly:

- `200`
