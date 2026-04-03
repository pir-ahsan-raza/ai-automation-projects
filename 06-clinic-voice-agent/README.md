# 06 — Clinic Voice Agent (AI Receptionist)

AI voice receptionist for Clinic-X built with VAPI + n8n + Twilio WhatsApp.

## What it does
- Answers inbound calls and schedules appointments via voice
- Checks real-time availability from Google Sheets
- Sends WhatsApp confirmation via Twilio
- Logs all calls to Google Sheets

## Stack
| Layer | Tool |
|---|---|
| Voice agent | VAPI |
| LLM | Groq — GPT OSS 120B |
| Transcriber | Deepgram Nova 3 Medical |
| TTS | OpenAI TTS-1-HD (Shimmer) |
| Automation | n8n |
| Confirmation | Twilio WhatsApp |
| Database | Google Sheets |

## VAPI Configuration
See [`vapi/settings.md`](./vapi/settings.md) for full setup.
Prompt is in [`vapi/prompt.txt`](./vapi/prompt.txt).

## n8n Workflow
Import [`n8n/workflow.json`](./n8n/workflow.json) directly into n8n.

### Workflow nodes
1. Vapi Webhook → Parse Event → Dynamic Dates → Route Event
2. **Slots branch:** Read Appointments → Calculate Slots → Respond Slots
3. **Booking branch:** Double Booking Guard → Normalize Phone → Save Appointment → Send WhatsApp → Respond Booked
4. **Call log branch:** Log Call → Respond OK

## Setup
1. Import workflow JSON into n8n
2. Connect Google Sheets OAuth credential
3. Add Twilio credential (Account SID + Auth Token)
4. Set your Twilio WhatsApp number in the Send WhatsApp node
5. Paste prompt into VAPI assistant
6. Point VAPI tool webhook URL to your n8n webhook

## Notes
- Pakistani number format (0300xxxxxxx) auto-converts to +92 E.164
- Caller's number used as fallback if patient skips phone input
- Double booking guard prevents race condition on same slot!
