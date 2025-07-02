# 🔄 MyApp Repository Restore

Restores your GitHub repository from secure backups stored in MyApp File Security with end-to-end encryption and OTP verification.

## ⚠️ Note
This is designed for empty repositories — it will overwrite all history

---

## 🚀 Quick Start

```yaml
name: Restore Repository
permissions: write-all

on:
  workflow_dispatch:
    inputs:
      otp_code:
        description: 'OTP CODE - Use it as blank for first run. Get OTP code via email and run again with OTP code. OTP code expires after 90 seconds'
        required: false

jobs:
  restore:
    runs-on: ubuntu-latest
    steps:
      - name: Restore Repository
        uses: berkayy-atas/marketplace-test-workflow-restore@latest
        with:
          activation_code: ${{ secrets.ACTIVATION_CODE }}
          encryption_key: ${{ secrets.ENCRYPTION_KEY }}
          otp_code: ${{ github.event.inputs.OTP_CODE }}
          record_id: 'string'
```

##  🚀 How to Use in a Blank Repository
Follow these steps to set up and run the restore action successfully:

1️⃣ Create a New Blank Repository
  - Go to GitHub – New Repository
  - Create a new empty repository


2️⃣ Create Required Folders
  - If working locally:

```bash
mkdir -p .github/workflows
```
  - Or if using the GitHub web UI:
  Click "Add file" > "Create new file"
  - Name it: .github/workflows/restore.yml

3️⃣ Add the Workflow File
  - Paste the following content into restore.yml:

```yaml
name: Restore Repository
permissions: write-all

on:
  workflow_dispatch:
    inputs:
      otp_code:
        description: 'OTP CODE - Use it as blank for first run. Get OTP code via email and run again with OTP code. OTP code expires after 90 seconds'
        required: false

jobs:
  restore:
    runs-on: ubuntu-latest
    steps:
      - name: Restore Repository
        uses: berkayy-atas/marketplace-test-workflow-restore@latest
        with:
          activation_code: ${{ secrets.ACTIVATION_CODE }}
          encryption_key: ${{ secrets.ENCRYPTION_KEY }}
          otp_code: ${{ github.event.inputs.OTP_CODE }}
          record_id: 'string'
```
4️⃣ Add Required Secrets
  - Go to: Repository → Settings → Secrets and variables → Actions
  - Click "New repository secret" and add the following:

Secret Name	Description
  - ACTIVATION_CODE:	Your API activation token from File Security
  - RESTORE_GITHUB_TOKEN:	A personal access token (PAT) with repo and workflow access (you can claim your token: "Github Settings -> Developer Settings -> Personal access tokens (classic)") [Scopes: repo, workflow, admin:org, write:discussion] 
  - ENCRYPTION_KEY: 32+ character decryption key (used during backup)
  - record_id: RecordId of the repository version you want to restore. You can find this id in the web UI or in the action outputs of the repository you backed up.

5️⃣ Run the Workflow
  - Go to the Actions tab in your GitHub repository
  - Select Restore Repository
  - Click "Run workflow" (Run it by leaving the otp value blank during the first run)
  
  ![image](https://github.com/user-attachments/assets/d408926d-262b-403f-8160-dd0baffd911b)

  - Check your email for the OTP code
  - Click "Run workflow" (Use it with the otp code you received)

  ![image](https://github.com/user-attachments/assets/22cf483a-a5e8-40e8-aaab-30e8f9542105)

  - The workflow will resume and complete the restore
 

# 🔑 Personal Access Token (PAT) Setup Guide for Repository Restoration

## Step 1: Create a Personal Access Token
1. Log in to your GitHub account
2. Click on your profile picture in the top-right corner
3. Navigate to: Settings → Developer Settings → Personal Access Tokens → Tokens (classic)
4. Click `Generate new token` then `Generate new token (classic)`

## Step 2: Configure Token Permissions
Set these required permissions:
Note: "Repository-Restore-Token"  # Example name
Expiration: 30 days             # Recommended duration
Permissions:
- repo       # Select ALL repository permissions
- workflow   # Required for workflow restoration

##Step 3: Add Token to Repository Secrets
1. In your repository, go to: Settings → Secrets → Actions
2. Click New repository secret`
3. Enter details:

```bash
Name: RESTORE_PAT_TOKEN  # This will be used in workflow
Secret: [Paste your generated token here]
```
Step 4: Configure Workflow File

Add this to your restoration workflow (.github/workflows/restore.yml):

```yaml
- name: Restore Repository with Workflows
  uses: berkayy-atas/marketplace-test-workflow-restore@v1
  with:
    restore_github_token: ${{ secrets.RESTORE_PAT_TOKEN }}  # Critical for workflows
    activation_code: ${{ secrets.ACTIVATION_CODE }}
    encryption_key: ${{ secrets.ENCRYPTION_KEY }}
```

## Features
- ✅ Two-factor authentication with OTP via email
- 🔐 AES-256 encrypted backup files
- ⏳ 90-second OTP expiration for security
- 🔄 Full repository mirroring (branches, tags, history)
- ⚙️ Optional workflow restoration

