# n8n Automation Journey 🤖

Learning n8n to build automated social media workflows.

## Day 1 — April 13, 2025
- Installed Docker Desktop on Windows 10
- Set up WSL2
- Got n8n running locally at localhost:5678
- Created n8n account

## Goal
Build an automated pipeline:
Google Drive video → AI generates title + caption → auto posts to YouTube + Instagram



## Day 2 — April 14, 2026
- Explored n8n template library
- Found perfect template: "From Google Drive to Instagram, TikTok & YouTube with AI descriptions"
- Learned how the workflow works: Google Drive trigger → AI generates description → posts to all 3 platforms automatically
- Set up Airtable for Project 1 (M) with video tracking table
- Set up Airtable for Project 2 (L) with video tracking table
- Stack decided: n8n + Google Gemini (free) + Airtable + direct APIs
- Total cost: $0


## Day 3 — April 16, 2026
- Loaded template "From Google Drive to Instagram, TikTok & YouTube with AI descriptions" into n8n
- Set up Google Cloud project (M & L) with OAuth2
- Enabled Google Drive API in Google Cloud Console
- Configured OAuth consent screen and added test user
- Fixed 403 access_denied error by adding Gmail to test users
- Successfully connected Google Drive OAuth2 credential in n8n
- Created Airtable Personal Access Token and connected it in n8n
- Template fully loaded with credentials set up for M project
- Next: swap OpenAI node for Gemini and map all workflow nodes
