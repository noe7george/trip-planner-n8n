# 🌍 TravelOS — AI Trip Planner Bot

An AI-powered Telegram bot that generates personalized, day-wise travel 
itineraries using **n8n**, **Google Gemini 2.5 Flash**, and the **Google 
Places API**.

Built as a no-code/low-code automation project demonstrating real-world 
LLM orchestration, API integration, and conversational UX design.

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🗣️ **Natural Language Parsing** | Extracts trip details from free-text messages using Gemini |
| 🧠 **Smart Field Detection** | Knows exactly which fields are missing and asks for them |
| 📸 **Attraction Discovery** | Fetches top-rated attractions from Google Places API |
| 🖼️ **Photo Carousel** | Sends 5 real attraction photos in one Telegram media group |
| 🎨 **AI Welcome Message** | Gemini writes a themed intro based on the destination |
| 📅 **Day-Wise Itinerary** | Generates a full schedule with times, costs, and activities |
| 💾 **Google Sheets Storage** | Persists trips and activities for recall |
| 🔁 **Conversational Recall** | Type "show itinerary" or "day 2" to view saved plans |

---

## 🏗️ Architecture

```
Telegram Message
       │
       ▼
┌──────────────┐
│   Switch     │  ← Routes by intent (welcome / show / day N / new trip)
└──────┬───────┘
       │
       ▼
┌──────────────────────┐
│ Gemini Extractor     │  ← Parses trip details from natural language
└──────┬───────────────┘
       │
       ▼
┌──────────────────────┐
│ Is Data Complete?    │
└──────┬───────────────┘
       │
   ┌───┴───┐
   │       │
  Yes      No
   │       │
   ▼       ▼
┌─────┐  ┌──────────────┐
│HTTP │  │ Ask for more │
│Places│ └──────────────┘
└──┬──┘
   │
   ▼
┌──────────────────────┐
│ Gemini Itinerary     │  ← Builds the day-wise plan
└──────┬───────────────┘
       │
       ▼
┌──────────────────────┐
│ Google Sheets        │  ← Saves trips + activities
└──────────────────────┘
```

---

## 🛠️ Tech Stack

- **[n8n](https://n8n.io/)** — Workflow automation
- **Google Gemini 2.5 Flash** — Natural language understanding & generation
- **Google Places API (New)** — Attraction search with ratings, photos, and reviews
- **Google Sheets API** — Persistent storage
- **Telegram Bot API** — User interface

---

## 📸 Demo

### 1. Welcome Message
![Welcome](screenshots/01_telegram_welcome.png)

### 2. Missing Data Prompt
![Missing Data](screenshots/02_missing_data_prompt.png)

### 3. Attraction Photos
![Attractions](screenshots/03_attraction_photos.png)

### 4. AI-Generated Summary
![Summary](screenshots/04_itinerary_summary.png)

### 5. Day-Wise View
![Day View](screenshots/05_day_wise_view.png)

### 6. Workflow Canvas
![Workflow](screenshots/06_workflow_diagram.png)

---

## 🚀 How to Use This Workflow

### Prerequisites
- A running n8n instance (self-hosted or [n8n Cloud](https://n8n.io/cloud))
- A Telegram bot (create via [@BotFather](https://t.me/BotFather))
- Google Cloud project with:
  - **Gemini API** enabled
  - **Places API (New)** enabled
  - **Sheets API** enabled
- A Google Sheet with two tabs:
  - **Trips**: `Trip ID | Trip Name | Destination | Country | Start Date | 
    End Date | Number of Days | Budget | Currency | Travelers | 
    Trip Style | Status | Created At`
  - **Activities**: `Trip ID | Trip Name | Day Number | Activity Order | 
    Start Time | End Time | Activity Name | Description | Category | 
    Location | Latitude | Longitude | Estimated Cost | Indoor | 
    Status | Notes`

### Setup Steps
1. Download `workflow/Trip_Planner_V2_workflow.json`
2. In n8n: **Workflows → Import from File** → select the JSON
3. Update the following in the imported workflow:
   - **Telegram credentials** → your bot token
   - **Google Gemini credentials** → your Gemini API key
   - **Google Sheets credentials** → connect your Google account
   - **HTTP Request node** → replace `YOUR_GOOGLE_PLACES_API_KEY` 
     with your actual key
   - **Photos node** → replace `YOUR_TELEGRAM_BOT_TOKEN` 
     with your actual bot token
   - **Google Sheets nodes** → update `documentId` to your Sheet's ID
4. Click **Save** → toggle **Active** ON

### Test the Bot
Send any of these to your Telegram bot:
- `/start` or `hi` → Welcome message
- `Plan a trip from Kochi to Munnar by car from 30/07/2026 to 06/08/2026 
  for 2 people with ₹100000` → Full itinerary
- `show itinerary` → Latest saved trip
- `day 2` → Day 2 activities
- `home` → Return to main menu

---

## 🔐 Security Note

This public repo contains a **sanitized** version of the workflow. 
All API keys and tokens have been replaced with placeholders like 
`YOUR_GOOGLE_PLACES_API_KEY`.

**Never commit live credentials.** Use n8n's credential system or 
environment variables instead.

---

## 📂 Project Structure

```
trip-planner-n8n/
├── README.md
├── workflow/
│   └── Trip_Planner_V2_workflow.json
└── screenshots/
    ├── 01_telegram_welcome.png
    ├── 02_missing_data_prompt.png
    ├── 03_attraction_photos.png
    ├── 04_itinerary_summary.png
    ├── 05_day_wise_view.png
    └── 06_workflow_diagram.png
```

---

## 🎯 What This Project Demonstrates

- **LLM orchestration** — Combining Gemini extraction + Gemini generation
- **Structured output parsing** — Forcing valid JSON from an LLM
- **Conditional workflows** — Different branches based on completeness
- **External API integration** — Google Places, Telegram, Sheets
- **Conversational UX** — Intent routing via the Switch node
- **Data persistence** — Read/write across Google Sheets
- **Error handling** — Graceful prompts for missing data

---

## 🛣️ Roadmap

- [ ] Add weather API integration for daily forecasts
- [ ] Support multi-language itineraries
- [ ] Export itinerary as PDF
- [ ] Add hotel and restaurant suggestions via Google Places
- [ ] Voice message input via Whisper
- [ ] Cost estimation with currency conversion

---

## 📜 License

MIT — free to use, modify, and share.

## 👤 Author

**Noel George Mathew**
- GitHub: [@noe7george](https://github.com/noe7george)

---

⭐ If you found this useful, give it a star!
