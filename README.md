# 📽️ YouTube Video Transcript & Highlight Summarization Workflow

This `n8n` workflow automates the entire process of:
1. **Fetching new YouTube video links from Google Sheets**
2. **Downloading the video via `yt-dlp`**
3. **Extracting transcripts via an external API**
4. **Generating highlight segments using AI (Gemini)**
5. **Summarizing content**
6. **Updating Google Sheets with transcripts and AI summaries**

---

## 🔗 Google Sheets Integration

- The workflow is triggered every minute using **Google Sheets Trigger**.
- It monitors the sheet for new YouTube video URLs.
- Trigger Sheet: [`YT Summary`](https://docs.google.com/spreadsheets/d/12bES542itCTPTs32jopyiYc1UUjm3jmfK70f3VIqGxQ/edit#gid=0)

---

## ⚙️ Workflow Overview

### Step 1: Trigger on New Entry
- **Node**: `Google Sheets Trigger`
- Watches for newly added video URLs.

### Step 2: Download Video
- **Node**: `Execute Command`
- Uses `yt-dlp` to download the video with restricted filename format.
- Example Command:
- 
### Step 3: Extract File Path
- **Node**: `Extract`
- Parses the output to get the final merged video path.

### Step 4: Transcript Generation
- **Node**: `HTTP Request`
- Sends the video URL to a transcript API (e.g., Apify or custom endpoint).
- Receives a transcript with timestamps.

---

## 🧠 AI Processing

### Step 5: Prompt Construction
- **Node**: `Code1`
- Formats the transcript data into a structured prompt for the AI summarizer.

### Step 6: Gemini Language Model (Optional)
- **Node**: `Google Gemini Chat Model`
- Linked with `LangChain Agent` to power intelligent highlight extraction.

### Step 7: LangChain Agent
- **Node**: `AI Agent`
- Given the transcript, it returns the top 5 highlights as structured JSON.

### Step 8: JSON Parser
- **Node**: `Code3`
- Converts AI output string to structured JSON array.

### Step 9: Highlight Splitter
- **Nodes**: `Split Out3` and `Split Out4`
- Splits highlights into individual items for processing and summarization.

### Step 10: Summarization
- **Node**: `Summarize1`
- Aggregates the highlight summaries into a concise overview using concatenation.

---

## 📄 Sheet Update (Final Output)

### Step 11A: Store Transcript
- **Node**: `Google Sheets`
- Updates the original video URL row with the cleaned transcript.

### Step 11B: Store Summary
- **Node**: `Google Sheets1`
- Updates the same row with the AI-generated content highlights summary.

---

## ✅ Features

- 📥 Auto-triggered on new YouTube video links
- 🧠 Gemini AI + LangChain integration
- 📄 Google Sheets input/output
- 🎯 Highlight detection with start/end timestamps
- 🧹 Transcript cleaning & summarization
- 🕒 Polls every minute for near-real-time processing

---

## 📁 File Structure

```text
📂 C:/Users/chall/Downloads/videos/
  └── Clean, safe-filename videos saved via yt-dlp
📋 Google Sheet Columns:
  ├── URL (input)
  ├── Transcript (output)
  └── Summary (output)

