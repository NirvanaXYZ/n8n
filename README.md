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

- ## Day 4 — April 27, 2026
- Deleted OpenAI node and replaced it with Google Gemini (Message a model)
- Selected Gemini model manually by ID: gemini-1.5-flash (not in dropdown list)
- Wrote social media prompt for Instagram, TikTok and YouTube captions
- Fixed Set Variables node: cleaned up Airtable IDs that were corrupted by Airtable AI assistant
- Configured Create Airtable Record node with all 9 field mappings:
  - Video Name, Status, Google Drive Link, File ID, Upload Date
  - Description (from Gemini output), Instagram Status, TikTok Status, YouTube Status
- Next: finish remaining nodes (Edit Airtable Fields, Update with Description, upload nodes)

## Day 5 — April 20, 2026
- Discovered old Airtable node version hides field labels causing silent failures
- Replaced "Create Airtable Record" with new "Create a record" node
- Fixed Airtable Personal Access Token — base was missing from token access
- New node loaded all Airtable columns automatically with proper labels and dropdowns
- Configured all 9 fields correctly in new Create a record node
- Fixed "Edit Airtable Fields1" — corrected wrong node references from
  "Google Drive" → "Read video from Google Drive"
- Replaced old "Update Airtable with Description" with new "Update record" node
- Configured Update record node with all fields including record ID chaining
- Next: Instagram, TikTok, YouTube upload nodes + full workflow test
