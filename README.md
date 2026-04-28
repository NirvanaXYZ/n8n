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


## Day 7 — April 24, 2026

### Goal: Replace YouTube Upload Node & Fix Airtable Nodes in YouTube Row

**Replacing the YouTube Upload Node:**
- Original node used upload-post.com (paid service — not available)
- Deleted old HTTP Request node pointing to upload-post.com API
- Added native n8n YouTube node (YouTube → Upload a video)
- Enabled YouTube Data API v3 in Google Cloud Console (existing project)
- Reused existing OAuth 2.0 Client — added n8n OAuth Callback URI
- Connected YouTube OAuth2 credential inside n8n successfully

**YouTube node configuration:**
- Operation: Upload
- Title: dynamic from Set Variables node
- Input Binary Field: `datavideo` (binary data from Read Video for YouTube node)
- Category: Education
- Region Code: US (required field — cannot be left empty)
- Description: dynamic from Set Variables node
- Privacy Status: Public

**Edit Airtable Fields 3 (Set Node):**
- Reviewed node — already correctly configured, no changes needed
- Contains: ID, YouTube Status, Youtube URL fields
- Note: This is a Set node, not an Airtable node — no replacement needed

**Replacing Update YouTube Status - Success Node:**
- Old node had broken expressions referencing deleted nodes
- Replaced with new Airtable → Update record node (new version auto-loads columns)
- Base: selected from list
- Table: Videos
- Columns to match on: id
- YouTube Status: Posted

**Challenges faced:**
- YouTube OAuth2 does not reuse Google Drive credentials — requires separate credential setup
- YouTube Data API v3 must be enabled separately in Google Cloud Console
- Old expressions referenced "Update Airtable with Description" node which no longer exists in workflow
- New Airtable node uses dropdown for YouTube Status instead of free text — correct value: Posted
- Youtube URL column does not exist in Airtable table — skipped

---

## Day 8 — April 27, 2026

### Goal: Complete X (Twitter) & Pinterest Nodes + Attempt Instagram API Setup

**Adding new Airtable columns:**
- Added X Status — Single Select (Pending, Posted, Failed)
- Added Pinterest Status — Single Select (Pending, Posted, Failed)
- Added TikTok URL — URL type
- Added YouTube URL — URL type
- Added Instagram URL — URL type
- Added X URL — URL type
- Added Pinterest URL — URL type

**Setting up X (Twitter) Developer Account:**
- Went to console.x.com and registered developer account
- Created new app for the automation project
- Set App permissions to Read and Write
- Set Type of App to Web App, Automated App or Bot
- Added n8n OAuth Callback URI in app settings
- Generated OAuth 2.0 Client ID and Client Secret
- Connected credential inside n8n successfully

**X (Twitter) nodes added:**
- Create Tweet — X native n8n node
- Edit Airtable Fields X — Set node (ID, X Status, X URL)
- Update X Status — Airtable Update Record node

**Tweet content template:**
- Video title from Set Variables node
- AI generated description from Set Variables node
- YouTube video link
- Hashtags: #M #aifashion #design

**Important note:** X API free plan does NOT support video upload — post text tweet with YouTube link instead. Video upload requires Basic paid plan ($100/month)

**X Challenges faced:**
- Initial Access Token was Read-only — had to change App permissions to Read and Write and regenerate token
- YouTube URL not accessible from X branch (different workflow branch) — fixed by referencing YouTube Status update node output instead of YouTube upload node directly

**Setting up Pinterest:**
- Converted Pinterest account from Personal to Business (Content Creator type)
- Went to developers.pinterest.com and created new app
- Trial access approved immediately
- Added n8n OAuth Callback URI in Redirect URIs
- Generated access token (Production Limited)
- Created dedicated Pinterest board for video content

**Pinterest nodes added:**
- Create Pinterest Pin — HTTP Request node (no native Pinterest node in n8n)
- Edit Airtable Fields Pinterest — Set node (ID, Pinterest Status, Pinterest URL)
- Update Pinterest Status — Airtable Update Record node

**Pinterest pin strategy:**
- Pinterest Trial API does NOT support video upload
- Used YouTube thumbnail as pin image: `https://img.youtube.com/vi/[VIDEO_ID]/maxresdefault.jpg`
- Pin links directly to YouTube video
- More visual approach — better engagement than plain link

**Pinterest Challenges faced:**
- No native Pinterest node in n8n — had to use HTTP Request node with manual Header Auth
- Board ID must be in `username/board-slug` format not numeric ID
- Trial access has limited scopes — write access requires proper OAuth flow

**Instagram API — BLOCKED ❌**
- Instagram video upload via Meta Graph API requires verified Facebook Developer account
- Confirmed: Instagram account is Business type ✅
- Confirmed: Instagram connected to Facebook Page ✅
- SMS verification failed — codes not received reliably
- Virtual/temporary phone numbers blocked by Meta entirely
- Credit card verification not available for this account
- business.facebook.com → Apps → Create new app ID button was greyed out
- Root cause: Meta blocks all alternative verification methods — requires a real verified phone number

**Workarounds to try next session:**
- Try verification using Facebook mobile app
- Try a different real phone number
- Try credit card verification if it becomes available

---

### Current Workflow Status

| Platform | Status |
|----------|--------|
| YouTube | ✅ Complete |
| X (Twitter) | ✅ Complete |
| Pinterest | ✅ Complete |
| Instagram | ❌ Blocked — Meta API verification |
| TikTok | ⏳ Waiting for API review approval |



## Day 9 — April 28, 2026

### Goal: Fix Google Drive Trigger, Resolve TikTok API Rejection & Set Up Policy Pages

---

### TikTok API — Third Submission Attempt

**Rejection reason from second submission:**
- Missing Terms of Service accessible from homepage
- Missing Privacy Policy accessible from homepage
- Invalid Terms of Service and Privacy Policy links provided

**Solution — Created dedicated policy pages on custom subdomain:**
- Created new GitHub repository for hosting policy documents
- Enabled GitHub Pages on main branch
- Added CNAME DNS record in domain registrar pointing subdomain to GitHub Pages
- Configured custom domain for the policies subdomain
- SSL certificate issued automatically by GitHub Pages
- Created `terms.html` — full Terms of Service page with branded styling
- Created `privacy.html` — full Privacy Policy page with branded styling
- Created `index.html` — landing page linking to both policy pages

**Third TikTok submission:**
- Updated Terms of Service URL with new hosted page
- Updated Privacy Policy URL with new hosted page
- Submitted for review — Status: In Review ⏳

**Challenge — GitHub Pages serving HTML as plain text:**
- First attempts showed raw unstyled text instead of rendered HTML
- Root cause: content was pasted without proper HTML tags and DOCTYPE declaration
- Fixed by replacing file content with complete valid HTML

**Challenge — SSL certificate delay:**
- After adding custom domain, HTTPS was unavailable for ~15 minutes
- GitHub Pages automatically issues SSL certificate — required waiting period
- Enforce HTTPS checkbox became available after certificate was issued

---

### Google Drive Trigger — Fixed ✅

**Problem:**
- Google Drive Trigger credential was expired
- Error: authorization grant invalid, expired, or revoked

**Fix:**
- Reconnected Google Drive credential by re-authenticating with Google account
- Credential successfully refreshed ✅

**Additional fix — Updated trigger folder:**
- Old template was pointing to a folder that didn't exist
- Created new dedicated Google Drive folder for social media content
- Updated Google Drive Trigger node to watch the new folder
- Watch For: File Created
- Poll Mode: Every Minute

---

### Current Workflow Status

| Platform | Status |
|----------|--------|
| YouTube | ✅ Complete |
| X (Twitter) | ✅ Complete |
| Pinterest | ✅ Complete |
| Instagram | ❌ Blocked — Meta Developer verification |
| TikTok | ⏳ In Review — 3rd submission |
| Google Drive Trigger | ✅ Fixed and reconnected |

---
