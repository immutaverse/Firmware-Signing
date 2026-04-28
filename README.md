# Immutaverse Firmware Signing - Setup Guide

![Immutaverse Blue White Logo](https://github.com/user-attachments/assets/f8929d7f-94a1-43bf-b73f-29aff0fc3756)

If you can read this, Immutaverse has granted you collaborator access to this repository. This guide explains how to set up firmware signing for your repository.

Once setup is complete, the process is simple: commit a `.bin` firmware file with a specific commit message, and the signed firmware will automatically be pushed back to your repository.

---

## Before You Start

Make sure you have the following ready:

- **OpenSSL** installed on your local machine
  - Linux / macOS: usually pre-installed. Verify by running `openssl version`
  - Windows: install via [Git for Windows](https://gitforwindows.org/)
- Your **own GitHub repository** where firmware files will be uploaded and signed firmware will be returned

---

## Step 1 - Add the Workflow File

The workflow file is already available in this repository.

### Copy the Workflow File

1. Navigate to `.github/workflows/blank.yml` in this repository, or open it directly:

   ```
   https://github.com/immutaverse/Firmware-Signing/blob/main/.github/workflows/blank.yml
   ```

2. Click **Copy raw file** (top-right of the file view) to copy the workflow contents.

### Add It to Your Repository

1. Open your repository on GitHub
2. Go to the **Actions** tab
3. Click **New workflow**
4. Select **Set up a workflow yourself**
5. Delete the placeholder content
6. Paste the copied workflow content
7. Save the file as `firmware-signing.yml`
8. Click **Commit changes**

GitHub automatically stores the workflow inside `.github/workflows/`.

---

## Step 2 - Add Repository Secrets

Your repository requires two GitHub Actions secrets.

Navigate to: **Repository -> Settings -> Secrets and variables -> Actions -> New repository secret**

---

### Secret 1: IMT_TOKEN

This is your GitHub Personal Access Token (PAT). It allows the workflow to:

- Read your firmware file
- Push the signed firmware back to your repository
- Allow Immutaverse to notify you of signing status

#### Create Your PAT

1. Click your GitHub profile picture, then open **Settings**
2. Scroll down and open **Developer settings**
3. Open **Personal access tokens** then **Fine-grained tokens**
4. Click **Generate new token**
5. Give the token a name, for example: `IMT_TOKEN`
6. Under **Repository access**, select **Only select repositories**
7. Choose your repository
8. Under **Repository Permissions**, add **Contents** and set it to **Read and write**
9. Under **Account Permissions**, add **Email addresses** and set it to **Read-only**
10. Click **Generate token**
11. Copy the token immediately. GitHub will not show it again.

#### Add IMT_TOKEN as a Secret

1. Go to **Repository Settings -> Secrets and variables -> Actions**
2. Click **New repository secret**
3. Name: `IMT_TOKEN`
4. Value: your generated PAT
5. Click **Add secret**

---

### Secret 2: SIGN_KEY

This is your unencrypted private signing key.

#### Add SIGN_KEY as a Secret

1. Go to **Repository Settings -> Secrets and variables -> Actions**
2. Click **New repository secret**
3. Name: `SIGN_KEY`
4. Value: your unencrypted private signing key
5. Click **Add secret**

---

## Step 3 - Sign Your Firmware

After setup is complete, signing firmware is straightforward.

1. Open your GitHub repository
2. Navigate to the folder where you want to upload the firmware
3. Click **Add file -> Upload files**
4. Upload your `.bin` firmware file
5. In the commit message field, enter exactly:

   ```
   Unsigned Firmware
   ```

   The commit message must match exactly. Any variation will prevent the workflow from running.

6. Click **Commit changes**
7. Open the **Actions** tab to monitor workflow execution
8. Once completed, refresh the repository folder

The signed firmware will appear in the same folder.

**Example:**

```
your_firmware.bin -> signed_your_firmware.bin
```

---

## Important Notes

- Firmware files must use the `.bin` extension
- The push must be made to the `main` branch
- The commit message must be exactly `Unsigned Firmware`
- Any variation in the commit message will prevent the workflow from triggering
