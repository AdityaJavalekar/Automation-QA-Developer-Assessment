# Task 2 — n8n API Integration Workflow

**Candidate:** Aditya Javalekar  
**Workflow:** Hacker News Hourly Top 5 Digest  
**Timezone:** Asia/Kolkata

## Overview
This workflow retrieves Hacker News top-story IDs, fetches the details for the first ten items, ranks valid stories by points, and sends a digest containing the top five to Telegram. It runs every hour when activated.

## APIs and transformation
- **Source API:** `https://hacker-news.firebaseio.com/v0/topstories.json` returns top-story item IDs.
- **Enrichment API:** `https://hacker-news.firebaseio.com/v0/item/{id}.json` retrieves each story's title, author, score, comment count, and URL.
- **Code nodes:** normalize the ID response, request details for the first ten IDs, discard deleted/invalid/non-story records, sort by score descending, and keep the top five.

The official Hacker News API documentation describes the topstories endpoint and the item endpoint: https://github.com/HackerNews/API

## Conditional logic
The IF node checks whether the highest-scoring story has **more than 1,000 points**:
- True: sends a high-traction Telegram digest.
- False: sends the standard Telegram digest.

## Error handling
Both HTTP Request nodes use **Continue using error output**. Their error outputs are routed to a Code node that builds an error message, then to a Telegram alert node. Review the failed execution in n8n for full diagnostic details.

## Setup before running
1. Import `Task2_Workflow_Aditya_Javalekar.json` into n8n.
2. Open each Telegram node and select/create your **Telegram API credential** in n8n Credentials.
3. Replace `YOUR_TELEGRAM_CHAT_ID` with the destination chat ID in all three Telegram nodes. Do not put your bot token in the workflow JSON.
4. Execute the workflow manually once. Confirm the HTTP nodes return data and that a Telegram message arrives.
5. Activate the workflow to enable the hourly schedule.

## Submission evidence still to capture
- Screenshot 1: imported workflow canvas with all nodes and connections visible.
- Screenshot 2: successful execution showing top-five output and/or delivered Telegram message.

**Status:** Workflow JSON and documentation prepared. It has not been imported or executed in the candidate's n8n instance; Telegram credentials/chat ID and execution screenshots are therefore still required. Do not describe it as live-tested until a successful execution is recorded.
