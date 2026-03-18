# workflow-structurer-skill

An interactive workflow structuring assistant that progressively clarifies and decomposes a vague process into a structured, executable workflow document.

**Use cases**
- Turn verbal or tacit processes into clear, executable steps
- Align teams on inputs/outputs/ownership/risks/acceptance before collaboration
- Capture reusable workflow templates for automation or handoff

**Trigger phrases**
- `梳理流程`
- `整理流程`
- `梳理工作流程`
- `帮我梳理一下流程`
- `organize the workflow`
- `help me structure this process`

**Interaction flow**
- Phase 1 Goal clarification: confirm core problem and success criteria
- Phase 2 Step skeleton: list step names only, no details
- Phase 3 Step drill-down: fill Input, Output, Responsibilities, Risks, Validation, Rules
- Phase 4 Step summary: use the template and confirm with the user
- Phase 5 Handoff output: generate a structured summary for skill-creator
- Phase 6 Trigger skill-creator: after producing the handoff, the user can say “创建技能” to create the skill

**Output template**
- Step summary template: `references/step-template.md`

**Key rules**
- Only handle one step at a time
- Never assume missing details; ask explicitly
- Identify Risks before writing Rules
- Never mix Validation and Rules

**Quick start**
1. Place the skill folder under `~/.codex/skills/workflow-structurer`
2. Restart Codex to load the skill
3. Start a new session and use a trigger phrase

**Example dialogue (full | link-summary-to-notes case)**
```
User: Organize a workflow
Assistant: What core problem are you trying to solve?
User: Automatically analyze article links, summarize, and store them in my notes
Assistant: What final result do you want?
User: After storing, return the summary
Assistant: From input to output, what are the main steps? Which step is most uncertain or error-prone?
User: 1) Fetch link content 2) Analyze and summarize the article 3) Open my notes app and create a note 4) Return the result and summary

Assistant: Let’s start with Step 1 (Fetch link content). Please fill the 6 fields: input, output, responsibilities, risks, validation, rules.
User:
1. Article link
2. Article content/link/title/author/platform
3. Content retrieval
4. Network issues, fetch failure
5. Able to obtain the main text
6. Must obtain the main text to count as success; DOM-only doesn’t count. If too long, take only the first 6000 characters.

Assistant:
## Step 1: Fetch Link Content

**Input:**
- Article link

**Output:**
- Article content
- Link
- Title
- Author
- Platform

**Responsibilities:**
- Retrieve content

**Risks:**
- Network issues causing fetch failure

**Validation:**
- Main text is obtained

**Rules:**
- Must obtain main text; DOM-only does not count
- If too long, keep only the first 6000 characters

Assistant: Is this step accurate?
User: Yes

Assistant: Completed: Step 1 (Fetch link content)
Assistant: Current: Step 2 / 4
Assistant: Please fill Step 2 (Analyze and summarize the article) with the 6 fields.
User:
1. Article content, link, title, author, platform
2. Summary conclusions
3. Analyze content and summarize
4. Summary may be incorrect
5. Use the summary to check whether it can cover the main text
6. Summary length must be no more than 500 characters

Assistant:
## Step 2: Analyze and Summarize the Article

**Input:**
- Article content
- Link
- Title
- Author
- Platform

**Output:**
- Summary conclusions

**Responsibilities:**
- Analyze content and generate summary

**Risks:**
- Summary is incorrect

**Validation:**
- Use the summary to check whether it can cover the main text

**Rules:**
- Summary length must be no more than 500 characters

Assistant: Is this step accurate?
User: Continue

Assistant: Completed: Step 1 (Fetch link content), Step 2 (Analyze and summarize)
Assistant: Current: Step 3 / 4
Assistant: Please fill Step 3 (Open notes app and create a note) with the 6 fields.
User:
1. Summary, link, title, author, platform
2. Note link
3. Create a new note
4. Errors creating the note; authorization timeout requires re-login
5. Able to obtain the share link of the new note
6. None

Assistant:
## Step 3: Open Notes App and Create a Note

**Input:**
- Summary
- Link
- Title
- Author
- Platform

**Output:**
- Note link

**Responsibilities:**
- Create a new note and write the content

**Risks:**
- Errors creating the note
- Authorization timeout requires re-login

**Validation:**
- Able to obtain the share link of the new note

**Rules:**
- None

User: The notes app URL should be configured in advance; we should validate it before all steps
Assistant: Do you want “notes app URL configured” to be Step 0?
User: Step 0

Assistant: Please fill Step 0 with the 6 fields.
User:
1. Notes app create-page URL
2. Notebook/knowledge base name
3. Save URL and check if it opens and permissions are valid
4. Opened page is not the create page; not logged in
5. Confirm it is a notebook/knowledge base page and you can create notes
6. If configuration fails, terminate

Assistant:
## Step 0: Pre-check (Notes App Configuration)

**Input:**
- Notes app create-page URL

**Output:**
- Notebook/knowledge base name

**Responsibilities:**
- Save the notes app URL
- Check accessibility and permissions

**Risks:**
- Opened page is not the create page
- Not logged in, cannot create

**Validation:**
- Confirm it is a notebook/knowledge base page
- Notes can be created on the page

**Rules:**
- If configuration fails, terminate the workflow

Assistant: Is this step accurate?
User: Yes

Assistant: Completed: Step 0 (Pre-check), Step 1, Step 2, Step 3
Assistant: Current: Step 4 / 4
Assistant: Please fill Step 4 (Return result and summary) with the 6 fields.
User:
1. Note share link
2. Result (share link) and summary
3. Present the result
4. Not likely to fail
5. Have share link and summary
6. None

Assistant:
## Step 4: Return Result and Summary

**Input:**
- Note share link

**Output:**
- Result (share link)
- Summary

**Responsibilities:**
- Present the result

**Risks:**
- Not likely to fail

**Validation:**
- Share link is present
- Summary is present

**Rules:**
- None
```

**Example result (Skill Handoff)**
```
# Skill Handoff: Link Summary to Notes

## Goal
Automatically analyze article links, generate a summary, store it in the notes app, and return the summary and note link.

## Trigger Phrases
- Summarize this link and save it to my notes
- Save this article to notes and return the summary
- Link summary to notes

## Steps
1. Pre-check (Notes app configuration): verify create-page URL is accessible, permissions are valid, and notes can be created; confirm target notebook.
2. Fetch link content: extract main text and core metadata.
3. Analyze and summarize: generate a summary and validate it can cover the main text.
4. Create note: write summary and metadata, then obtain share link.
5. Return result: return share link and summary.

## Key Rules
- If configuration fails, terminate the workflow
- Must obtain main text; DOM-only does not count
- If too long, keep only the first 6000 characters
- Summary length must be no more than 500 characters

## Key Risks
- Network issues causing fetch failure
- Summary is incorrect
- Note creation fails or auth expires requiring re-login

## Validation Summary
- Create-page is accessible and notes can be created
- Main text is obtained
- Summary can cover the main text
- Share link and summary are returned

## Suggested Skill Name
link-summary-to-notes
```
