# AGENT GUIDELINES - DEVELOPMENT

## Before implementation

Always perform code analysis first:
1. Look for similar functionality already implemented in the project
2. Identify existing patterns, conventions, and architectural approaches
3. Review similar features to understand how they are structured
4. Ask: "Are there existing files or examples in the project I should follow as reference?"

### Reuse instead of duplication

- Extend existing functionality rather than creating parallel implementations
- Propose refactoring similar code when patterns emerge
- Leverage existing service layers, repositories, and components

---

## During plan implementation

### Search for existing examples and patterns

Before implementing each plan phase:

1. Check whether similar code already exists
- Search for analogous implementations in the codebase
- Identify architectural patterns used for similar use cases
- Analyze how similar problems were solved in the past

2. Questions to ask continuously:
- "Is this functionality already implemented elsewhere in the project?"
- "Which files or modules can I use as examples?"
- "How were similar features implemented?"
- "Are there existing utilities or helpers I can reuse?"
- "What is the standard architectural pattern in this project for this kind of operation?"

3. Iterative approach:
- For each task in the plan, search for examples first
- Adapt the implementation to existing patterns
- Reuse components, services, or existing logic
- Maintain consistency with the project's architectural style

4. When no examples are found:
- Explicitly state that you are creating a new pattern
- Document architectural choices
- Propose the approach before implementation
- Consider whether the new pattern could be useful for future refactoring

Example workflow:
```
Task: Implement email notifications for orders
↓
1. Search: "Are there already email notifications implemented?"
2. Find: Notification system for user registration
3. Analyze: How is it structured? Which classes/services does it use?
4. Reuse: Extend the existing service instead of creating a new one
5. Implement: Following the established pattern
```

---

## Testing

The agent always proposes automated tests for new features or critical changes when relevant, and at the end of each implementation step.

---

## Generic technical principles

Non-prescriptive guidelines to use as reference:

- Separate responsibilities (UI/logic/data)
- Maintain clear interfaces between components
- Write testable, decoupled code
- Reuse existing patterns in the project
- Flag testing needs when relevant
