# n8n AI Recruitment Pipeline

An automated job application screening workflow built with n8n. When someone submits an application, this pipeline uses OpenAI to read and score their CV, then automatically schedules an interview for strong candidates or sends a polite rejection to weaker ones. All candidates are logged to Airtable and your team gets a Slack notification for every shortlist.

## What it does

A candidate submits their name, email, role, and CV text to a webhook. The workflow validates the data, sends the CV to an OpenAI assistant for scoring, and routes the candidate down one of two paths based on the AI recommendation.

Shortlisted candidates get a Google Calendar interview booked two days out, an invitation email from Gmail, a record created in Airtable with their score, and a Slack message to your team.

Rejected candidates get a polite email and a record in Airtable marked as rejected.

## Tools used

OpenAI for CV screening and scoring, Google Calendar for interview scheduling, Gmail for sending emails, Airtable for candidate tracking, and Slack for team notifications.

## Workflow overview

Webhook intake receives the application data. A validation step checks all required fields are present. The AI screener evaluates the CV and returns a score between 0 and 100 along with a recommendation of SHORTLIST or REJECT. A decision node splits the flow based on that recommendation.

## Sample request

Send a POST request to your webhook URL with this JSON body:

```json
{
  "name": "Jane Smith",
  "email": "jane@example.com",
  "role": "Senior Backend Engineer",
  "cv_text": "5 years experience in Python and Node.js..."
}
```

A successful response looks like this:

```json
{
  "status": "success",
  "message": "Application processed"
}
```

## License

MIT
