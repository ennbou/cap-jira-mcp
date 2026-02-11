---
applyTo: '**'
---

# Jira Backlog Management Instructions

## Project Context
- **Project**: SmartCook AI - Flutter Mobile Application
- **Type**: Mobile App (iOS/Android)
- **Tech Stack**: Flutter, Dart, Provider, OpenRouter API

## Issue Types & Templates

### 📱 Feature (Story)
Use for new user-facing functionality.

**Template:**
```
Summary: [Feature] <Short descriptive title>

Description:
## User Story
As a [user type],
I want to [action],
So that [benefit].

## Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Technical Notes
- Affected screens: 
- Providers involved:
- AI Service integration: Yes/No

## UI/UX
- Figma reference: [link if available]
- Responsive considerations:
```

**Labels**: `feature`, `mobile`, `flutter`

---

### 🐛 Bug
Use for defects and issues.

**Template:**
```
Summary: [Bug] <Short descriptive title>

Description:
## Environment
- Device: 
- OS Version:
- App Version:

## Steps to Reproduce
1. Step 1
2. Step 2
3. Step 3

## Expected Behavior
What should happen.

## Actual Behavior
What actually happens.

## Screenshots/Logs
[Attach relevant screenshots or error logs]

## Severity
- [ ] Critical (App crash, data loss)
- [ ] High (Feature broken)
- [ ] Medium (Feature partially working)
- [ ] Low (Minor issue)
```

**Labels**: `bug`, `mobile`

---

### 🔧 Technical Task
Use for technical improvements, refactoring, or infrastructure.

**Template:**
```
Summary: [Tech] <Short descriptive title>

Description:
## Objective
What needs to be done technically.

## Reason
Why this is needed (performance, maintainability, etc.).

## Implementation Details
- Files affected:
- Dependencies:
- Breaking changes: Yes/No

## Definition of Done
- [ ] Code implemented
- [ ] Tests added/updated
- [ ] Documentation updated
- [ ] Code reviewed
```

**Labels**: `technical`, `flutter`

---

### 🎨 UI/UX Task
Use for design implementation or UI improvements.

**Template:**
```
Summary: [UI] <Short descriptive title>

Description:
## Screen/Component
Name of the screen or widget.

## Design Reference
- Figma link:
- Screenshots:

## Requirements
- [ ] Responsive design (mobile sizes)
- [ ] Dark mode support
- [ ] Accessibility (a11y)

## Widgets to Create/Modify
- Widget 1:
- Widget 2:
```

**Labels**: `ui`, `design`, `flutter`

---

## SmartCook AI - Feature Areas

When creating issues, use these components/labels to categorize:

| Component | Description | Folder |
|-----------|-------------|--------|
| `ingredients` | Ingredient detection & management | `lib/features/ingredients/` |
| `recipes` | Recipe generation & display | `lib/features/recipes/` |
| `home` | Home screen & navigation | `lib/features/home/` |
| `camera` | Photo capture functionality | `lib/features/ingredients/screens/camera_screen.dart` |
| `ai-service` | AI/OpenRouter integration | `lib/core/services/` |
| `core` | Shared models, theme, widgets | `lib/core/` |

---

## Priority Guidelines

| Priority | Criteria | Example |
|----------|----------|---------|
| **Highest** | Blocks release, critical bug | App crash on launch |
| **High** | Core feature, major bug | Photo detection not working |
| **Medium** | Important feature, moderate bug | Recipe display formatting |
| **Low** | Nice-to-have, minor enhancement | Animation improvements |
| **Lowest** | Future consideration | Dark mode support |

---

## Story Points (Fibonacci)

| Points | Complexity | Time Estimate |
|--------|------------|---------------|
| 1 | Trivial | < 2 hours |
| 2 | Simple | 2-4 hours |
| 3 | Moderate | 4-8 hours |
| 5 | Complex | 1-2 days |
| 8 | Very Complex | 2-3 days |
| 13 | Epic-level | 3-5 days |

---

## Sprint Workflow

### Status Transitions
1. **Backlog** → Ready for sprint planning
2. **To Do** → Sprint committed
3. **In Progress** → Development started
4. **Code Review** → PR submitted
5. **Testing** → QA validation
6. **Done** → Merged & deployed

---

## MCP Tools Usage

### Creating Issues
Use `jira_create_issue` with:
- `project_key`: Your Jira project key
- `summary`: Follow templates above
- `issue_type`: Story, Bug, Task, Sub-task
- `description`: Use markdown formatting

### Searching Issues
Use `jira_search` with JQL:
```
project = "PROJECT_KEY" AND status != Done ORDER BY priority DESC
```

### Linking Issues
Use `jira_link_issues` to connect:
- **blocks** / **is blocked by**
- **relates to**
- **is parent of** / **is child of**

---

## Best Practices

1. **One ticket = One deliverable** - Keep issues focused
2. **Clear acceptance criteria** - Define "done" explicitly
3. **Link related issues** - Maintain traceability
4. **Update status promptly** - Keep board accurate
5. **Add labels consistently** - Enable filtering
6. **Estimate before sprint** - Use planning poker
7. **Include screen references** - Link to Figma/designs
8. **Document blockers** - Flag dependencies early

---

## SmartCook AI - Epic Structure

### Epic 1: Ingredient Management
- Photo capture functionality
- AI-powered ingredient detection
- Manual ingredient input
- Ingredient list editing (add/remove/modify)

### Epic 2: Recipe Generation
- AI recipe generation from ingredients
- Recipe list display
- Recipe detail view
- Recipe filtering/sorting

### Epic 3: User Experience
- Home screen dashboard
- Navigation flow
- Loading states & error handling
- Onboarding experience

### Epic 4: Core Infrastructure
- AI service integration (OpenRouter)
- State management setup
- Theme & styling system
- Shared widgets library



## Usage with MCP Tools
When you want to create tickets, you can use the Jira MCP tools. For example:

- Create a feature: Use activate_jira_issue_management_tools then jira_create_issue
- Search backlog: Use activate_jira_issue_query_tools then jira_search
- Manage sprints: Use activate_jira_sprint_management_tools
