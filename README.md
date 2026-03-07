# N8N-Email-Assistant

# How to install N8N for desktop
Watch following video on how to install N8N on desktop for free. 
https://www.youtube.com/watch?v=mJkvTP1q70s


# Description of template

This template is for any user who wants an email assistant to sort out and manage their GMAIL inbox 
including classifying emails into different categories, drafting responses to emails and extracting 
financial information from simple invoices received to save time and manual effort to keep 
your GMAIL inbox tidy and manageable.

# How it works ?

Email Checking: 
Every hour the workflow checks for unread emails.

Classification: 
Gemini analyses incoming messages that are unread and classifies them into different categories.

Invoice processing: 
Emails that contain an invoice attachment go through invoice processing. 
Gemini analyses, extracts the pdf, uploads it to google sheets with the relevant data extracted

Smart respones: 
For emails that require replies the workflow checks if it is business hours or weekend hours. 
During weekeend and off hours it sends an automatic reply. 
During business hours it creates a draft response for you ready to view and send.

Setup Instructions

Step 1:

Google cloud: Go to https://console.cloud.google.com/ and create a new project. 
Enable required API services Gmail API, Google Drive API and Google Sheets API

Step 2:

Create OAUTH credentials: Follow instructions then save client key and secret key

Step 3: 

Visit https://docs.n8n.io/integrations/builtin/credentials/sendemail/gmail/#generate-an-app-password
and follow instructions on how to set up SMPT

Step 4:

In Google Drive, create a folder called “Invoice Storage” and copy its folder ID from the URL to input into the drive node. 
Create a Google Sheet called “Reconciliation Sheet” with four column headers: “Invoice date”, “Invoice Description”, “Total price”, and “Document”. 
Copy the spreadsheet ID from its URL to input into the sheets node. In Gmail settings, create the lables that you need for email classification.

Step 5:

Visit https://aistudio.google.com/ then click on the get api key and save it

Step 6:

Download the json file from the repository and import into an n8n instance or 
copy and paste the code from the json file. 
Configure three credentials using the IDs and keys you saved: 
Gmail OAuth2 (Client ID + Secret), 
Google Drive OAuth2 (same Client ID + Secret), 
Google Gemini API (API key), 
connect each credential in n8n when prompted and follow the prompt instructions and authorize access.
