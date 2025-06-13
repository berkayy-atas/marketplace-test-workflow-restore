# 🔄 MyApp Repository Restore

Restores your GitHub repository from secure backups stored in MyApp File Security with end-to-end encryption and OTP verification.

## ⚠️ Note
This is designed for empty repositories — it will overwrite all history

---

## 🚀 Quick Start

```yaml
name: Restore Repository

on:
  workflow_dispatch:
    inputs:
      file_record_id:
        description: 'File Record ID'
        required: true

jobs:
  restore:
    runs-on: ubuntu-latest
    permissions: write-all
    steps:
      - name: Restore Repository
        uses: berkayy-atas/marketplace-test-workflow-restore@latest
        with:
          activation_code: ${{ secrets.ACTIVATION_CODE }}
          github_token: ${{ secrets.RESTORE_GITHUB_TOKEN }}
          file_record_id: ${{ github.event.inputs.file_record_id }}
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

on:
  workflow_dispatch:
    inputs:
      file_record_id:
        description: 'File Record ID'
        required: true

jobs:
  restore:
    runs-on: ubuntu-latest
    permissions: write-all
    steps:
      - name: Restore Repository
        uses: berkayy-atas/marketplace-test-workflow-restore@latest
        with:
          activation_code: ${{ secrets.ACTIVATION_CODE }}
          github_token: ${{ secrets.RESTORE_GITHUB_TOKEN }}
          file_record_id: ${{ github.event.inputs.file_record_id }}
```
4️⃣ Add Required Secrets
  - Go to: Repository → Settings → Secrets and variables → Actions
  - Click "New repository secret" and add the following:

Secret Name	Description
  - ACTIVATION_CODE:	Your API activation token from File Security
  - RESTORE_GITHUB_TOKEN:	A personal access token (PAT) with repo and workflow access (you can claim your token: "Github Settings -> Developer Settings -> Personal access tokens (classic)")

5️⃣ Run the Workflow
  - Go to the Actions tab in your GitHub repository
  -Select Restore Repository
  - Click "Run workflow" (top right)
  - Fill in FILE_RECORD_ID

6️⃣ Enter OTP When Prompted
  - The workflow will pause and wait for you to provide the OTP
  - Check your email for the OTP code
  - Paste it into the GitHub UI field when prompted
  - The workflow will resume and complete the restore

✅ What This Action Does
  - Securely retrieves an activation token and sends an OTP
  - Downloads and extracts your backup
  - Pushes the full mirror to your GitHub repository

