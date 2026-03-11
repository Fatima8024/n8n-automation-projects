# AI Voice Scheduling Agent

An AI-powered voice scheduling assistant built with **n8n**, **Azure OpenAI**, **Google Sheets**, **Google Calendar**, **Gmail**, and **ElevenLabs**.

This project acts like a virtual receptionist that can understand user requests, find contact details, schedule meetings, and send confirmation emails automatically.

---

## Features

- Accepts user requests through a webhook
- Uses an AI Agent powered by Azure OpenAI
- Looks up contact details from Google Sheets
- Creates calendar events in Google Calendar
- Sends confirmation emails through Gmail
- Connects with ElevenLabs for voice-based interaction

---

## Workflow Overview

The workflow follows this architecture:

**ElevenLabs Voice Agent → n8n Webhook → AI Agent → Google Sheets / Google Calendar / Gmail → Respond to Webhook**

### Main flow:
1. A user speaks to the voice agent
2. ElevenLabs sends the request to the n8n webhook
3. The AI Agent understands the request
4. It uses tools when needed:
   - **Google Sheets** to find contact details
   - **Google Calendar** to schedule meetings
   - **Gmail** to send confirmations
5. The workflow returns a response
6. The voice agent speaks the result back to the user

---

## Tools Used

- **n8n**
- **Azure OpenAI Chat Model**
- **Google Sheets**
- **Google Calendar**
- **Gmail**
- **ElevenLabs**
- **ngrok** (for HTTPS webhook exposure during testing)

---

## Example Use Cases

- “Find Sophia’s email”
- “Schedule a meeting with Sophia tomorrow at 3 PM”
- “Book an appointment and send both of us an email confirmation”

---

## Challenges Faced

This project involved several real-world integration challenges, including:

- Setting up HTTPS access for local n8n webhooks using ngrok
- Connecting ElevenLabs webhook tools to n8n
- Debugging `404 Not Found` webhook errors
- Fixing AI Agent iteration limits in n8n
- Ensuring the calendar event was actually created before confirming success
- Passing the correct structured input (`chatInput`) from ElevenLabs to n8n

These debugging steps were a big part of the learning process.

---

## What I Learned

Building AI agents is not just about prompts — it is also about:

- reliable webhook integration
- tool orchestration
- structured input/output design
- debugging multi-service workflows
- patience and persistence

---

## Future Improvements

- Replace the ElevenLabs layer with a custom **Pipecat** voice agent
- Add memory/session handling for multi-turn scheduling conversations
- Improve availability checking before booking
- Upload the full Pipecat-integrated version to GitHub

---

## Files in This Folder

- `workflow.json` — exported n8n workflow
- `README.md` — project documentation
- `screenshot.png` — workflow preview

---

## Author

Built by Fatima Javed as part of an AI automation and voice agent learning journey.
