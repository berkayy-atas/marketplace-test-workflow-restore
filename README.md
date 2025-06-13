# 🔄 MyApp Repository Restore

Restores your GitHub repository from secure backups stored in MyApp File Security with end-to-end encryption and OTP verification.

---

## 🚀 Quick Start

```yaml
name: Restore Repository

on:
  workflow_dispatch:
    inputs:
      file_record_id:
        description: 'Backup File ID'
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
