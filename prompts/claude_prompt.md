# Claude Code - Suggested Prompts by Category

> Extracted and categorized from: Common workflows - Claude Code Docs

---

## 1. UNDERSTANDING CODEBASES

Prompts for getting a quick overview and deep-diving into code structure:

- `give me an overview of this codebase`
- `explain the main architecture patterns used here`
- `what are the key data models?`
- `how is authentication handled?`

---

## 2. FINDING & LOCATING CODE

Prompts for locating specific features, files, and understanding component interactions:

- `find the files that handle user authentication`
- `how do these authentication files work together?`
- `trace the login process from front-end to database`

---

## 3. BUG FIXING

Prompts for identifying, understanding, and fixing bugs:

- `I'm seeing an error when I run npm test`
- `suggest a few ways to fix the @ts-ignore in user.ts`
- `update user.ts to add the null check you suggested`

---

## 4. REFACTORING CODE

Prompts for modernizing and improving existing code:

- `find deprecated API usage in our codebase`
- `suggest how to refactor utils.js to use modern JavaScript features`
- `refactor utils.js to use ES2024 features while maintaining the same behavior`
- `run tests for the refactored code`

---

## 5. USING SUBAGENTS

Prompts for leveraging specialized AI subagents for specific tasks:

- `/agents` (view available subagents)
- `review my recent code changes for security issues`
- `run all tests and fix any failures`
- `use the code-reviewer subagent to check the auth module`
- `have the debugger subagent investigate why users can't log in`

---

## 6. PLAN MODE (Complex Analysis)

Prompts for creating detailed plans before implementing changes:

- `I need to refactor our authentication system to use OAuth2. Create a detailed migration plan.`
- `What about backward compatibility?`
- `How should we handle database migration?`
- `Analyze the authentication system and suggest improvements`

---

## 7. REQUIREMENTS GATHERING (Interview Mode)

Prompts for having Claude ask clarifying questions:

- `Interview me about this feature before you start: user notification system`
- `Help me think through the requirements for authentication by asking questions`
- `Ask me clarifying questions to build out this spec: payment processing`

---

## 8. TESTING

Prompts for identifying, generating, and running tests:

- `find functions in NotificationsService.swift that are not covered by tests`
- `add tests for the notification service`
- `add test cases for edge conditions in the notification service`
- `run the new tests and fix any failures`

---

## 9. PULL REQUESTS

Prompts for creating and reviewing pull requests:

- `summarize the changes I've made to the authentication module`
- `create a pr`
- `enhance the PR description with more context about the security improvements`
- `add information about how these changes were tested`

---

## 10. DOCUMENTATION

Prompts for generating and improving code documentation:

- `find functions without proper JSDoc comments in the auth module`
- `add JSDoc comments to the undocumented functions in auth.js`
- `improve the generated documentation with more context and examples`
- `check if the documentation follows our project standards`

---

## 11. IMAGE & VISUAL ANALYSIS

Prompts for analyzing images, screenshots, diagrams, and design mockups:

- `What does this image show?`
- `Describe the UI elements in this screenshot`
- `Are there any problematic elements in this diagram?`
- `Here's a screenshot of the error. What's causing it?`
- `This is our current database schema. How should we modify it for the new feature?`
- `Generate CSS to match this design mockup`
- `What HTML structure would recreate this component?`

---

## 12. FILE & DIRECTORY REFERENCES

Prompts for referencing and analyzing files and directories:

- `Explain the logic in @src/utils/auth.js`
- `What's the structure of @src/components?`
- `Show me the data from @github:repos/owner/repo/issues`

---

## 13. EXTENDED THINKING / ARCHITECTURAL DECISIONS

Prompts for deep reasoning and complex problem-solving:

- `ultrathink: design a caching layer for our API`

---

## 14. VERIFICATION & LINTING (Unix-style)

Prompts for code review and quality checks:

- `you are a linter. please look at the changes vs. main and report any issues related to typos. report the filename and line number on one line, and a description of the issue on the second line. do not return any other text.`

---

## 15. CAPABILITY QUESTIONS

Prompts for asking Claude about its features and limitations:

- `can Claude Code create pull requests?`
- `how does Claude Code handle permissions?`
- `what slash commands are available?`
- `how do I use MCP with Claude Code?`
- `how do I configure Claude Code for Amazon Bedrock?`
- `what are the limitations of Claude Code?`

---

## Summary

**Total: 63 distinct suggested prompts** across **15 categories**

This collection covers all major workflows in Claude Code development including:
- Code analysis and understanding
- Bug fixing and debugging
- Refactoring and modernization
- Testing and quality assurance
- Documentation
- Visual content analysis
- Planning and requirements gathering
