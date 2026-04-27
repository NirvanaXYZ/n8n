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

- ## Day 4 — April 17, 2026
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


## Day 6 — April 22, 2026

### Goal: Apply for TikTok Content Posting API

**First attempt — Hit a paywall:**
- Tried to use upload-post.com (the service used in the n8n template)
- Discovered TikTok integration requires a paid plan ($16/month minimum)
- Decided to apply directly for the official TikTok Content Posting API instead

**Creating the TikTok Developer App:**
- Went to developers.tiktok.com
- Created app with ownership type: Individual
- App name: M
- App type: Other (covers Content Posting API)

**Domain Verification problems:**
- App requires verified Terms of Service URL and Privacy Policy URL
- Tried using Instagram profile URL — rejected (not verified)
- Tried using project domain — got "Invalid domain" error because included https://
- Discovered two verification methods: URL prefix (needs file upload) and Domain (DNS record)
- URL prefix method failed — needed FTP/hosting access to upload verification file
- Switched to Domain verification via DNS TXT record
- Went to domain registrar (get.art) → DNS Management → Manage TXT Records
- Added TXT record with Host: @ and Value: tiktok-developers-site-verification code
- Domain verified successfully ✅

**Adding Products and Scopes:**
- Added Login Kit first (required before Content Posting API)
- Added Content Posting API (grayed out until Login Kit was added)
- Enabled Direct Post toggle (required to unlock video.publish scope)
- Final scopes added:
  - user.info.basic (included in Login Kit)
  - video.publish (included in Content Posting API - Direct Post)
  - video.upload (included in Content Posting API)
- Set Redirect URI to project domain

**App Review submission:**
- Filled app description explaining n8n automation workflow
- Uploaded demo video showing the integration
- Submitted for review
- Status: In Review ✅

**Saved credentials:**
- Client Key and Client Secret saved for later use in n8n

**Next steps:**
- Wait 2-3 weeks for TikTok API approval
- Continue with YouTube upload node (free, already have Google OAuth)
- Then Instagram upload node
- Come back to TikTok node once approved

Day 8 — April 27, 2026
Goal: Complete X (Twitter) & Pinterest Nodes + Attempt Instagram API Setup

X (Twitter) Node — Completed ✅
Setting up X Developer Account:

Went to console.x.com and registered developer account
Created a new app for the automation project
Set App permissions to Read and Write
Set Type of App to Web App, Automated App or Bot
Added n8n OAuth Callback URI in the app settings
Generated OAuth 2.0 Client ID and Client Secret
Connected credential inside n8n successfully

Nodes added to workflow:

Create Tweet — X native n8n node
Edit Airtable Fields X — Set node
Update X Status — Airtable Update Record node

Tweet content template:
[Video Title]
[AI Generated Description]
🎥 Watch on YouTube: [YouTube URL]
#hashtag1 #hashtag2 #hashtag3
Important note: X API free plan does NOT support video upload. We post a text tweet with YouTube link instead. Video upload requires Basic paid plan ($100/month).
Challenge — X Access Token was Read-only:

Initial token only had Read permissions
Had to go to Authentication Settings, change to "Read and Write", then regenerate token

Challenge — YouTube URL not accessible from X branch:

X branch splits from "Update record" node and cannot directly access the YouTube upload node output
Fixed by referencing the YouTube Status update node output instead


Pinterest Node — Completed ✅
Setting up Pinterest:

Converted Pinterest account from Personal to Business (Content Creator type)
Went to developers.pinterest.com and created a new app
Trial access approved immediately
Added n8n OAuth Callback URI in Redirect URIs
Generated access token (Production Limited)
Created a dedicated Pinterest board for video content

Important note: n8n does NOT have a native Pinterest node — used HTTP Request node instead
Nodes added to workflow:

Create Pinterest Pin — HTTP Request node
Edit Airtable Fields Pinterest — Set node
Update Pinterest Status — Airtable Update Record node

Create Pinterest Pin configuration:

Method: POST
URL: https://api.pinterest.com/v5/pins
Authentication: Generic Credential Type → Header Auth
Header: Authorization: Bearer [token]

Pin content strategy:

Pinterest Trial API does NOT support video upload
Used YouTube thumbnail as pin image: https://img.youtube.com/vi/[VIDEO_ID]/maxresdefault.jpg
Pin links directly to YouTube video
This approach is more visual and gets better engagement than a plain link

Challenge — No native Pinterest node in n8n:

Had to manually configure HTTP Request node with Header Auth
Board ID format must be username/board-slug not a numeric ID

Challenge — Pinterest Trial API limitations:

Trial access only provides limited read scopes by default
Write access for creating pins requires proper OAuth flow with write scopes
Video upload requires Standard access approval


Instagram Node — BLOCKED ❌
Instagram video upload via Meta Graph API requires a verified Facebook Developer account. Spent significant time trying to complete verification but hit multiple blockers.
What was confirmed working:

Instagram account is a Business account ✅
Instagram account is connected to Facebook Page ✅

Verification attempts that failed:

SMS verification — codes not received reliably
Virtual/temporary phone numbers — blocked by Meta
Credit card verification — not available for this account type
business.facebook.com → Apps → "Create new app ID" was greyed out (requires verified developer account first)

Root cause: Facebook Developer registration requires SMS verification to a real phone number linked to the Facebook account. Meta blocks virtual/temporary numbers entirely.
Workaround options for next session:

Try verification using Facebook mobile app
Try a different real phone number
Try credit card verification if it becomes available


Airtable Updates
Added 7 new columns to Videos table to track all platforms:

X Status — Single Select (Pending, Posted, Failed)
Pinterest Status — Single Select (Pending, Posted, Failed)
TikTok URL — URL
YouTube URL — URL
Instagram URL — URL
X URL — URL
Pinterest URL — URL


Current Workflow Status
PlatformStatusYouTube✅ CompleteX (Twitter)✅ CompletePinterest✅ CompleteInstagram❌ Blocked — Meta API verificationTikTok⏳ Waiting for API review approval
