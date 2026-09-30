# CareNerve — Healthcare Lead Qualification with AI Voice Calls

An automated intake system for a home healthcare provider. It checks whether a service is available at the customer's pincode, calls qualified leads with an AI voice agent, turns the call transcript into structured lead data, and notifies the customer and internal teams.

**Built with:** n8n · ElevenLabs Conversational AI (outbound calls via Twilio) · OpenAI GPT-4.1-mini · Google Sheets · Gmail · Webhooks

---

## The Problem
Home healthcare providers get enquiries for services such as nursing, GDA (general duty assistant) care and physiotherapy, but not every service is available in every area. Staff spend time calling leads that can't be served, and qualified leads wait too long for a first call.

## What It Does
1. **Intake:** A website form webhook captures the lead and prepares the data.
2. **Serviceability check:** The pincode and requested service (Nursing, GDA or Physiotherapy) are checked against a service coverage sheet.
3. **Not serviceable:** The customer receives a polite rejection email explaining the service isn't available in their pincode.
4. **Serviceable:**
   - A unique lead ID is generated and the lead is logged.
   - An **ElevenLabs AI voice agent places an outbound call** to qualify the lead.
5. **Call results:** Webhooks receive the call data, and a separate parser workflow uses **GPT-4.1-mini** to turn the conversation transcript into structured fields.
6. **Lead processing:** The qualified lead is updated in Google Sheets, and emails go to the customer, the sales team and the operations team.

## Architecture
![Workflow diagram](diagram.png)

## Key Design Decisions
- **Filter before calling:** serviceability is checked first, so AI call minutes are only spent on leads that can actually be served.
- **Transcript to structured data:** an LLM parser converts free-form conversation into consistent fields the team can act on.
- **Modular workflows:** the master workflow and the conversation parser are separate, so either can be changed without breaking the other.

## Files
- `carenerve_master.json` — intake, serviceability check, AI call and notifications
- `carenerve_parser.json` — AI call transcript parser and lead update
- `diagram.png` — architecture diagram

All credentials, phone numbers, ElevenLabs IDs and sheet IDs are replaced with placeholders.

## Setup
1. Import both workflows into n8n.
2. Connect Google Sheets, Gmail and OpenAI credentials, and add your ElevenLabs API key.
3. Replace `YOUR_ELEVENLABS_AGENT_ID`, `YOUR_ELEVENLABS_PHONE_NUMBER_ID` and `YOUR_GOOGLE_SHEET_ID` with your own values.
4. Point the parser's HTTP Request node at your own n8n webhook URL.

---
Built by **Pranita Priya**, n8n & AI Automation Builder
