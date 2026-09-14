---
description: Build lead capture site, wire n8n email plus Sheets, deploy to GitHub plus Vercel in 3 stages
agent: build
---

Build a full-stack lead capture website in 3 gated stages. Work in order. Do not skip ahead without explicit user approval via the question tool.

Business description from user: $ARGUMENTS
If $ARGUMENTS is empty, ask for the business type and goal before starting.

Global rules for all stages:
- Use todowrite to track stage progress.
- First load skill frontend-design and follow it for layout, type, and restraint.
- For any n8n work, first invoke the relevant n8n skills.
- Keep the project to simple HTML, CSS, vanilla JS unless the user asks otherwise.
- Copy must be direct, punchy, conversion-focused in the style of Alex Hormozi: clear value, strong hook, specific benefits, concise messaging, strong CTA, focus on customer outcome. Do not use em dashes or bullet characters in copy. Do not add useless structural comments in code.
- Never guess field names, IDs, or URLs. Read the actual files.
- All work happens inside the new project folder created in Stage 0, not the current directory root.

STAGE 0 - Create project folder:
1. Derive a short kebab-case folder name that fits the business description and give it a good name.
2. Use the question tool to propose that name and allow the user to rename. Only proceed on explicit confirm.
3. Create it with bash mkdir -p in the current working directory, then run all later stages inside that folder. If a folder with that name already exists, STOP and ask for a different name.
4. Report the full path created.

STAGE 1 - Frontend, iterate until happy:
1. Inside the new project folder from Stage 0 create:
  - index.html for main page
  - styles.css for styling
  - main.js for interactions
2. Include a lead form with name, email, phone, message. On submit send the data with fetch to a placeholder constant called N8N_WEBHOOK_URL.
3. Open the created index.html in the user's browser for review. Then STOP.
4. Use the question tool to ask: happy to proceed to backend, or keep iterating on design or copy? Only proceed to Stage 2 on explicit happy.

STAGE 2 - Connect frontend to n8n:
Prerequisites check first. STOP if missing and show how to fix:
- Gmail and Google Sheets connected in Composio
- Google Sheets OAuth2 API and Gmail OAuth2 API credentials set up in n8n

Then:
1. Read index.html and main.js to get the real form field names. Do not assume names.
2. Using n8n MCP for all n8n objects:
  - Create a workflow with a good fitting name based on the project, with Webhook trigger accepting POST for those exact fields
  - Add Send Email step that emails the user the new lead details
  - Add Google Sheets step that appends the same lead plus timestamp. Create the sheet via Composio MCP with a good fitting name if none exists.
3. Publish the workflow, get the production webhook URL, and automatically update main.js to replace N8N_WEBHOOK_URL. Do not ask the user to do this manually.
4. Submit a test lead yourself via the workflow. Then using Composio MCP only for verification, confirm the email arrived and a new row with form fields plus timestamp appeared in Google Sheets.
5. Then STOP. Tell the user to test end to end themselves: fill the form on the site, check inbox, open Google Sheets for the new row. If anything is missing, ask them to paste the error so you can fix the workflow or fetch call. Use the question tool to ask: proceed to deploy, or fix backend?

STAGE 3 - Deploy to GitHub and Vercel:
Prerequisites check first. STOP if missing:
- GitHub and Vercel connected in Composio

Then:
1. Via Composio create a private GitHub repository with a good fitting name for this project, then commit and push with git.
2. Via Composio deploy to Vercel and ensure Vercel is connected to the GitHub repository for automatic deployment.
3. Verify the live URL with webfetch and report it plus what was wired: form to n8n to email and Sheets.
