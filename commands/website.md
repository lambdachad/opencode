---
description: Build website with SEO, wire n8n automation, deploy to GitHub plus Vercel in 3 stages
agent: build
---

Build a full-stack website in 3 gated stages. Work in order. Do not skip ahead without explicit user approval via the question tool.

Business description from user: $ARGUMENTS
If $ARGUMENTS is empty, ask for the business type and goal before starting.

Global rules for all stages:
- Use todowrite to track stage progress.
- First load skill frontend-design and follow it for layout, type, and restraint.
- For any n8n work, first invoke the relevant n8n skills.
- Keep the project to simple HTML, CSS, vanilla JS unless the user asks otherwise.
- Copy must be direct, punchy, conversion-focused in the style of Alex Hormozi: clear value, strong hook, specific benefits, concise messaging, strong CTA, focus on customer outcome. Do not use em dashes or bullet characters in copy. Do not add useless structural comments in code.
- Never guess field names, IDs, or URLs. Read the actual files.
- All work happens inside the new project folder created in Stage 1, not the current directory root.

STAGE 1 - Website with SEO, iterate until happy:
1. Derive a short kebab-case folder name that fits the business description and give it a good name.
2. Use the question tool to ask what kind of website to build (for example lead capture, booking, quote, contact, portfolio) and allow a custom type. Also propose the folder name and allow rename. Only proceed on explicit confirm for both.
3. Create it with bash mkdir -p in the current working directory, then run all later work inside that folder. If a folder with that name already exists, STOP and ask for a different name. Report the full path created.
4. Inside the new project folder create:
  - index.html for main page
  - styles.css for styling
  - main.js for interactions
5. Build the page for the chosen site type. If the site needs a form, include fields that fit its purpose and on submit send the data with fetch to a placeholder constant called N8N_WEBHOOK_URL.
6. Include optimal SEO by default as part of the website:
  - Descriptive title under 60 chars plus meta description under 160 chars based on the business
  - Viewport, charset, canonical, robots meta, theme-color
  - Open Graph plus Twitter card tags
  - One h1, logical h2 order, semantic header main section footer, descriptive alt text on images
  - JSON-LD LocalBusiness schema with name, phone, area served, and services from the page copy
  - Favicon link plus robots.txt plus sitemap.xml with the main page, relative paths so Vercel serves them
7. Open the created index.html in the user's browser for review. Then STOP.
8. Use the question tool to ask: happy to proceed to backend, or keep iterating on design, copy, or SEO? Only proceed to Stage 2 on explicit happy.

STAGE 2 - Connect website to n8n:
Prerequisites check first. STOP if missing and show how to fix:
- Required Composio connections plus n8n credentials for whatever services the chosen workflow needs (for example Gmail and Google Sheets connected in Composio plus matching OAuth2 credentials in n8n)

Then:
1. Use the question tool to suggest 2 to 3 good fitting n8n workflows for the chosen site type (for example lead capture with email plus Sheets, booking with email plus Sheets plus calendar, contact with email only) and allow a fully custom workflow. Only proceed on explicit pick.
2. Read index.html and main.js to get the real field names for whatever the form sends. Do not assume names. If the site has no form, skip the Webhook trigger and build the workflow standalone.
3. Using n8n MCP for all n8n objects, build the picked workflow with a good fitting name based on the project. Create sheets, docs, or other destinations via Composio MCP with a good fitting name if none exists.
4. Publish the workflow. If the frontend needs it, get the production webhook URL and automatically update main.js to replace N8N_WEBHOOK_URL. Do not ask the user to do this manually.
5. Submit a test yourself via the workflow. Then using Composio MCP only for verification, confirm the expected side effects happened (for example email arrived, new row with form fields plus timestamp appeared).
6. Then STOP. Tell the user to test end to end themselves and confirm the same side effects. If anything is missing, ask them to paste the error so you can fix the workflow or fetch call. Use the question tool to ask: proceed to deploy, or fix backend?

STAGE 3 - Deploy to GitHub and Vercel:
Prerequisites check first. STOP if missing:
- GitHub and Vercel connected in Composio

Then:
1. Via Composio MCP create a private GitHub repository with a good fitting name for this project, then commit and push with git.
2. Via Composio MCP deploy to Vercel and ensure Vercel is connected to the GitHub repository for automatic deployment.
3. Verify the live URL with webfetch and report it plus what was wired.
