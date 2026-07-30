# N8N Email Assistant

An n8n workflow that automatically sorts, replies to, and processes your Gmail inbox using Google Gemini.

## What it does

Every hour, the workflow checks your Gmail inbox for unread messages and:

1. **Classifies** each email into categories using Gemini (Google's AI model).
2. **Processes invoices** — if an email has an invoice attachment, Gemini reads the PDF, extracts the key details (date, description, amount), and logs them in a Google Sheet.
3. **Handles replies smartly**:
   - Outside business hours or on weekends → sends an automatic reply.
   - During business hours → creates a draft reply in Gmail for you to review and send.

## Before you start

You'll need free accounts/access for:

- A **Google account** (Gmail, Google Drive, Google Sheets)
- **Google Cloud Console** access (to create API credentials)
- **Google AI Studio** access (for a free Gemini API key)
- An **n8n instance** — either n8n Cloud (hosted for you) or a self-hosted instance (local machine or your own server)

---

## Step 1: Set up your n8n instance

You need somewhere to run the workflow. Pick whichever fits you — you don't need both.

### Option A — n8n Cloud (easiest, no installation)

1. Go to [n8n.io](https://n8n.io) and click **Get started for free**.
2. Sign up and create a workspace. n8n Cloud gives you a hosted instance with a URL like `yourname.app.n8n.cloud` — no setup needed.
3. Once your workspace loads, you're ready to import the workflow by downloading the json file and importing it into the workflow.

### Option B — Self-hosted, local install with Docker
Requires [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running.

Watch this video to install N8N using Docker: https://www.youtube.com/watch?v=mJkvTP1q70s

## Step 2: Create a Google Cloud project and enable APIs

1. Go to [console.cloud.google.com](https://console.cloud.google.com/) and create a new project (or select an existing one).
2. In the search bar, find and **enable** each of these APIs:
   - Gmail API
   - Google Drive API
   - Google Sheets API

## Step 3: Create OAuth credentials

1. In the same Google Cloud project, go to **APIs & Services → Credentials**.
2. Click **Create Credentials → OAuth client ID**.
3. If prompted, configure the **OAuth consent screen** first (choose "External" if you're not on Google Workspace, and add your own email as a test user).
4. Choose **Web application** as the application type.
5. Save the **Client ID** and **Client Secret** shown — you'll need both later when connecting Gmail and Google Drive in n8n.

## Step 4: Set up an SMTP app password for Gmail

This lets n8n send emails through your Gmail account.

1. Follow n8n's official guide here: [Gmail SMTP credential setup](https://docs.n8n.io/integrations/builtin/credentials/sendemail/gmail/#generate-an-app-password).
2. This walks you through generating a Gmail **App Password** (requires 2-Step Verification to be turned on for your Google account).
3. Save the generated app password — you'll enter it into n8n's SMTP credential later.

## Step 5: Prepare Google Drive, Google Sheets, and Gmail labels

1. **Google Drive**: create a folder named `Invoice Storage`. Open it and copy the folder ID from the browser URL (the string after `/folders/`).
2. **Google Sheets**: create a new spreadsheet named `Reconciliation Sheet` with these four column headers in row 1:
   - `Invoice date`
   - `Invoice Description`
   - `Total price`
   - `Document`
   
   Copy the spreadsheet ID from its URL (the string between `/d/` and `/edit`).
3. **Gmail labels**: in Gmail settings, create the labels you want emails classified into (e.g. "Important", "Newsletter", "Invoice", "Needs Reply").

## Step 6: Get your Gemini API key

1. Go to [aistudio.google.com](https://aistudio.google.com/).
2. Click **Get API key** and generate a new key.
3. Save the key somewhere safe — you'll paste it into n8n's Gemini credential.

## Step 7: Import the workflow into n8n

1. Download `An email assistant to manage your GMAIL ibox (2).json` from this repository.
2. In your n8n instance, go to **Workflows → Add workflow → Import from File**, and select the downloaded JSON file.
   - Alternatively, open the JSON file in a text editor, copy all the contents, and paste it directly into the n8n canvas (n8n will auto-create the workflow from pasted JSON).

## Step 8: Connect your credentials in n8n

The imported workflow will show nodes with a credential warning until you connect them. Set up these three credentials (n8n will prompt you when you click into each relevant node):

- **Gmail (OAuth2)** — enter the Client ID + Client Secret from Step 3.
- **Google Drive (OAuth2)** — enter the same Client ID + Client Secret from Step 3.
- **Google Gemini (API Key)** — enter the API key from Step 6.

For each OAuth2 credential, n8n will open a Google sign-in popup — log in and click **Allow** to authorize access.

## Step 9: Configure the workflow nodes

1. Open the **Google Drive** node and paste in your `Invoice Storage` folder ID from Step 5.
2. Open the **Google Sheets** node and paste in your `Reconciliation Sheet` spreadsheet ID from Step 5.
3. Open the **classification** node(s) and confirm the Gmail labels match the ones you created in Step 5.
4. Set up the **SMTP** credential using the app password from Step 4, so automatic replies can be sent.

## Step 10: Activate the workflow

1. Click **Save**.
2. Toggle the workflow to **Active** in the top-right corner.
3. The workflow will now run automatically once per hour, checking for unread emails.

---

## Troubleshooting tips

- **Credential errors**: double-check that all three Google APIs (Gmail, Drive, Sheets) are enabled in the same Cloud project used for your OAuth credentials.
- **No emails processed**: confirm the workflow is set to **Active**, and that there are genuinely unread emails in the inbox.
- **Replies not sending**: re-check your SMTP app password — these expire if 2-Step Verification settings change.
