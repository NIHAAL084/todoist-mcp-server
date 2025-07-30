# @nihaal084/todoist-mcp-server (ADK-Compatible)

**Fixed version with Google Gemini API compatibility**

An MCP (Model Context Protocol) server implementation that integrates AI assistants with Todoist, enabling natural language task management. This server allows AI models to interact with your Todoist tasks using everyday language.

## What's Fixed

This is a patched version of [@nihaal084/todoist-mcp-server](https://github.com/NIHAAL084/todoist-mcp-server) that resolves enum type validation issues with Google's Gemini API and Google ADK.

**Background:**
Google ADK enforces strict schema validation, and the original MCP server was using integers for fields like priority (e.g., 4), whereas ADK expected strings (e.g., "4"). This led to runtime errors that couldn’t be fixed with prompt engineering or wrapping the server with a custom toolset.

### Original Issue

The original server defined priority enums as `type: "number"` with integer values `[1, 2, 3, 4]`, causing validation errors with Gemini API.

### Fix Applied

- Changed priority field types from `"number"` to `"string"` in the input schemas for all tools
- Updated enum values to strings: `["1", "2", "3", "4"]`
- Added runtime conversion from strings back to integers when calling Todoist API
- Updated all tool schemas and handlers to match this change
- Ensured compatibility with Gemini, Claude, and other MCP clients

## Features

- **Natural Language Task Management**: Create, update, complete, and delete tasks using everyday language
- **Smart Task Search**: Find tasks using partial name matches
- **Flexible Filtering**: Filter tasks by due date, priority, and other attributes
- **Rich Task Details**: Support for descriptions, due dates, and priority levels
- **Intuitive Error Handling**: Clear feedback for better user experience

## Installation

```bash
npm install -g @nihaal084/todoist-mcp-server
```

## Tools

### todoist_create_task

Create new tasks with various attributes:

- Required: content (task title)
- Optional: description, due date, priority level (1-4, as string)
- Example: "Create task 'Team Meeting' with description 'Weekly sync' due tomorrow"

### todoist_get_tasks

Retrieve and filter tasks:

- Filter by due date, priority, or project
- Natural language date filtering
- Optional result limit
- Example: "Show high priority tasks due this week"

### todoist_update_task

Update existing tasks using natural language search:

- Find tasks by partial name match
- Update any task attribute (content, description, due date, priority)
- Example: "Update meeting task to be due next Monday"

### todoist_complete_task

Mark tasks as complete using natural language search:

- Find tasks by partial name match
- Confirm completion status
- Example: "Mark the documentation task as complete"

### todoist_delete_task

Remove tasks using natural language search:

- Find and delete tasks by name
- Confirmation messages
- Example: "Delete the PR review task"

## Setup

### Getting a Todoist API Token

1. Log in to your Todoist account
2. Navigate to Settings → Integrations
3. Find your API token under "Developer"

### Usage with Claude Desktop

Add to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "todoist": {
      "command": "npx",
      "args": ["-y", "@nihaal084/todoist-mcp-server"],
      "env": {
        "TODOIST_API_TOKEN": "your_api_token_here"
      }
    }
  }
}
```

## Example Usage

### Creating Tasks

```
"Create task 'Team Meeting'"
"Add task 'Review PR' due tomorrow at 2pm"
"Create high priority task 'Fix bug' with description 'Critical performance issue'"
```

### Getting Tasks

```
"Show all my tasks"
"List tasks due today"
"Get high priority tasks"
"Show tasks due this week"
```

### Updating Tasks

```
"Update documentation task to be due next week"
"Change priority of bug fix task to urgent"
"Add description to team meeting task"
```

### Completing Tasks

```
"Mark the PR review task as complete"
"Complete the documentation task"
```

### Deleting Tasks

```
"Delete the PR review task"
"Remove meeting prep task"
```

## Development

### Building from source

```bash
# Clone the repository
git clone https://github.com/NIHAAL084/todoist-mcp-server.git

# Navigate to directory
cd todoist-mcp-server

# Install dependencies
npm install

# Build the project
npm run build
```

## Contributing

Contributions are welcome! Feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
