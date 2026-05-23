# Setup Guide

Follow these steps to get the pipeline running in your own n8n instance.

## Step 1: Import the workflow

Download the workflow JSON file from this repo. In your n8n instance, go to Workflows, click the three dot menu in the top right, and choose Import from file. Select the JSON file and the workflow will be loaded.

## Step 2: Connect your accounts

You need to set up five credentials inside n8n. Go to Settings then Credentials to add each one.

**OpenAI**: Create a credential of type OpenAI API and paste your API key from platform.openai.com. You also need to have an assistant already created in your OpenAI account. Copy that assistant ID and paste it into the AI CV Screener node where it says assistantId.

**Google Calendar**: Create a credential of type Google Calendar OAuth2 API. Follow the OAuth flow to connect the Google account that owns the calendar you want interviews booked into.

**Gmail**: Create a credential of type Gmail OAuth2 API. Connect the Google account you want to send emails from. This can be the same account as Calendar.

**Airtable**: Create a credential of type Airtable Personal Access Token. Generate a token at airtable.com/create/tokens with read and write access to your base. Paste it into n8n. Then open the Save Shortlisted and Save Rejected nodes and update the base URL and table ID to match your own Airtable base.

**Slack**: Create a credential of type Slack OAuth2 API. Follow the OAuth flow to connect your Slack workspace. Then open the Notify Slack node and choose which user or channel should receive the notifications.

## Step 3: Set up your Airtable base

Your Airtable table needs these five columns exactly as named:

Name as a text field, Email as a text field, Role as a text field, Score as a number field, Status as a text field.

## Step 4: Activate the workflow

Toggle the workflow from inactive to active using the switch in the top right corner of the editor. Once active, your webhook URL will be live. You can find the full webhook URL by clicking the New Application Webhook node and copying the Production URL shown there.

## Step 5: Test it

Send a test POST request to your webhook URL using a tool like Postman or curl:

```bash
curl -X POST https://your-n8n-instance.com/webhook/job-application \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Test Candidate",
    "email": "test@example.com",
    "role": "Software Engineer",
    "cv_text": "10 years experience building scalable web applications in Python and Go."
  }'
```

You should see the application processed, an email sent, a calendar event created, and a new row in Airtable.

## Customising the AI prompt

Open the AI CV Screener node to edit the prompt. You can add specific skills to look for, change the scoring rubric, or adjust the threshold logic in the Shortlist Decision node. By default any recommendation of SHORTLIST proceeds down the shortlist path and anything else goes to reject.

## Troubleshooting

If the webhook returns a missing required fields error, check that your request body includes all four fields: name, email, role, and cv_text.

If the AI response fails to parse, open the Parse AI Response node and check the raw output from the AI CV Screener. The AI must return valid JSON only with no extra text or markdown fences.

If calendar events are not appearing, confirm that the Google Calendar credential has permission to write to the correct calendar.
