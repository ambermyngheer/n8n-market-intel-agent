# n8n-market-intel-agent

🤖 An Automated Industry Daily Digest (n8n + Gemini AI)

## 📋 Overview
This repository contains an **n8n workflow** designed to act as an autonomous Business Intelligence Analyst. It ingests news from multiple global RSS feeds, filters for the most recent high-impact articles, and utilizes **Google Gemini AI** to generate a strategic daily digest for executive teams.

The system is built to minimize "noise" by scoring articles against specific business strategic pillars and summarizing only the top 3 most impactful updates per category.

---

## 🏗️ Workflow Architecture
* **Trigger:** A `Schedule Trigger` initiates the workflow every morning at 8:00 AM.
* **Ingestion:** A `Code in JavaScript` node manages a customizable list of RSS feeds (e.g., TechCrunch, Reuters, HBR).
* **Collection & Validation:**
    * `RSS Read` fetches the raw data from all sources.
    * `Filter` ensures only articles published within the last 24 hours are processed.
* **Intelligence Layer:**
    * `Aggregate` bundles the filtered articles into a single payload.
    * `Basic LLM Chain` (powered by Gemini) analyzes the data, discards PR fluff, and categorizes news into Industry, Supply Chain, Tech Innovation, and Competitor moves.
* **Delivery:** The AI’s markdown output is converted to HTML and routed to your inbox via Gmail.

---

## 🚀 Setup Instructions

### 1. Import the Workflow
* Download the `Industry Daily Digest.json` file from this repository.
* In your n8n instance, click **Import from File** and select the JSON.

### 2. Configure Gemini AI Credentials
* Obtain a free API key from [Google AI Studio](https://aistudio.google.com/).
* In the n8n workflow, open the **Google Gemini Chat Model** node.
* Click **Create New Credential** and paste your API key.

### 3. Configure Email Delivery
The workflow uses the **Gmail node** (labeled "Send a message") to deliver the report.

* **Credential Setup:**
    1.  Open the Gmail node and select **Create New Credential**.
    2.  Choose **OAuth2** (easiest for personal or Google Workspace accounts) and follow the authorization prompts.
* **Updating the Recipient:**
    1.  Inside the Gmail node, locate the **To Email** field.
    2.  Replace the placeholder with your professional email address.
    3.  *(Optional)* Update the **Subject Line** to include your company name.

### 4. Inject Industry-Relevant RSS Feeds
To make this agent work for your specific niche, you need to provide it with high-quality news sources.
1.  **Find your feeds:** Look for RSS links on your favorite industry publications (usually found in the footer or by adding `/feed/` to the URL).
2.  **Update the Code Node:** Open the **Code in JavaScript** node at the start of the workflow.
3.  **Add your sources:** Locate the `const feeds = [...]` array and add your URLs following the existing schema:
```javascript
{
  "category": "Retail Strategy", 
  "publicationName": "Retail Dive",
  "rssFeedUrl": "[https://www.retaildive.com/feeds/news/](https://www.retaildive.com/feeds/news/)",
  "strategicFocus": "Direct-to-consumer trends and inventory management"
}
