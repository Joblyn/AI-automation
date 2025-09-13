# Overview  
This guide walks you through creating a new **LinkedIn Content Creator** workflow from scratch in **n8n**. The workflow uses a Google Sheet as input, searches the web for relevant articles using the Tavily API, and uses OpenAI (via LangChain) to generate a professional, concise, and inspiring LinkedIn post for entrepreneurs. The final post is saved back into the same Google Sheet.

## Context  
- Use case: Generate LinkedIn posts based on trending topics.  
- Input source: Google Sheet with `Topic` and `Status`.  
- Content generation: OpenAI (GPT-4.1 Mini via LangChain)  
- Article sourcing: Tavily Web Search API  
- Output destination: Google Sheet  

## Instructions  

### Step 1: Prepare Your Google Sheet  
1. Create a new Google Sheet with the following columns:  
   - `Topic` (e.g., "Future of Remote Work")  
   - `Status` (Set initial value to `To Do`)  
   - `Content` (leave blank)  

2. Share the sheet and ensure your n8n Google Sheets credentials have edit access.

---

### Step 2: Set Up Credentials in n8n  
1. Go to **Credentials** in the n8n sidebar.  
2. Add the following credentials:  
   - **Google Sheets OAuth2 API**  
   - **Tavily API**  
   - **OpenAI API** (used via LangChain)  

---

### Step 3: Create a New Workflow  
1. Open n8n and click **+ New Workflow**.  
2. Name your workflow: `LinkedIn Content Creator`.

---

### Step 4: Add Nodes  

#### 1. **Manual Trigger**  
- Node: `Manual Trigger`  
- Purpose: Manually trigger the workflow.  

#### 2. **Google Sheets - Get Topic**  
- Node: `Google Sheets`  
- Operation: `Lookup`  
- Filter: `Status` equals `To Do`  
- Sheet URL: Paste your sheet URL (See [Sheet Template](https://docs.google.com/spreadsheets/d/1C2UikHCHK0SNZN1oraQJW1SRQQ6lGfzNOPabKXhq8Ro/copy))  
- Return only the first match  

#### 3. **Tavily - Web Search**  
- Node: `Tavily Web Search`  
- Query: `Search the web for {{ $json.Topic }}`  
- Max Results: `3`  
- Include Images: `true` (optional)  

#### 4. **OpenAI Chat Model (LangChain)**  
- Node: `OpenAI Chat Model (LangChain)`  
- Model: `gpt-4.1-mini`  
- Add your OpenAI credentials  

#### 5. **AI Agent (LangChain Agent)**  
- Node: `LangChain AI Agent`  
- Prompt Input: Use this format:  
  ```
  Article 1: {{ $json.results[0].content }}
  Article 2: {{ $json.results[1].content }}
  Article 3: {{ $json.results[2].content }}
  ```  
- System Prompt:  
  Paste the full system prompt used for generating concise, motivational posts. (See [Prompt Template](/PromptTemplate.md))  
- Connect the **AI Agent** to the **OpenAI Chat Model** via the `ai_languageModel` output.

#### 6. **Google Sheets - Update Row**  
- Node: `Google Sheets`  
- Operation: `Update`  
- Match on: `Topic`  
- Set values:  
  - `Status`: `Created`  
  - `Content`: `{{ $json.output }}`  

---

### Step 5: Connect Nodes  
Connect the nodes in this order:  
1. **Manual Trigger** → **Get Topic**  
2. **Get Topic** → **Tavily Web Search**  
3. **Tavily Web Search** → **AI Agent**  
4. **AI Agent** → **Google Sheets - Update Row**  
5. **OpenAI Chat Model** → `ai_languageModel` on AI Agent  

---

### Step 6: Test the Workflow  
1. Add a test row to your Google Sheet with a sample `Topic` and `Status = To Do`.  
2. Click **Execute Workflow**.  
3. After completion, check your sheet — the `Content` column should be populated and `Status` updated to `Created`.

---

## Tools  
- n8n  
- Google Sheets  
- Tavily API  
- OpenAI GPT (via LangChain)  

---

## Examples  
**Input Google Sheet Row:**  
- Topic: "Mindful Productivity in Startups"  
- Status: `To Do`  

**Output LinkedIn Post:**  
"Mindfulness isn't a luxury in the startup grind—it’s a leadership superpower. Blending productivity with presence leads to sustainable innovation. 🚀  

Let's build companies that prioritize both speed and soul.  
#StartupMindset #Productivity #Mindfulness #Leadership"

---

## SOP (Standard Operating Procedure)  
1. Add new `Topic` rows in the Google Sheet and set `Status = To Do`.  
2. Run the workflow manually or on a schedule.  
3. Wait for article search, AI generation, and sheet update.  
4. Copy the generated post and publish on LinkedIn.  

---

## Final Notes  
- You can trigger the workflow on a schedule using a **Cron** node.  
- Add logic to skip or flag topics with insufficient search results.  
- Consider adding LinkedIn API integration to auto-publish posts.

---
```
