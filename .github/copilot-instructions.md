# Copilot Instructions for Mergington High School Activities API

## Project Overview
This is a simple FastAPI web application for managing high school extracurricular activities. Students can view available activities and sign up through a clean web interface.

## Architecture & Key Files

### Backend (`src/app.py`)
- **FastAPI application** with 3 main endpoints: root redirect, get activities, activity signup
- **In-memory data storage** using Python dictionaries - data resets on server restart
- **Static file serving** mounted at `/static` for frontend assets
- **Activity model**: `activities[activity_name]` with description, schedule, max_participants, participants list

### Frontend (`src/static/`)
- **Pure HTML/CSS/JavaScript** - no build tools or frameworks
- **`index.html`**: Single-page app with activities list and signup form
- **`app.js`**: Async/await API calls, DOM manipulation, form handling
- **`styles.css`**: Responsive design with flexbox, activity cards, form styling

### Development Setup
- **VS Code configuration**: `.vscode/launch.json` for debugging with uvicorn
- **Dev container**: `.devcontainer/devcontainer.json` with Python 3.13, port forwarding, Copilot extensions
- **Dependencies**: `requirements.txt` lists `fastapi` and `uvicorn`

## Developer Workflows

### Running the Application
```bash
# Install dependencies
pip install fastapi uvicorn

# Start server (from src/ directory)
uvicorn app:app --reload

# Or use VS Code debugger: "Launch Mergington WebApp"
```

### API Testing
- **Local server**: http://localhost:8000 (redirects to static frontend)
- **API docs**: http://localhost:8000/docs (auto-generated Swagger)
- **Endpoints**: 
  - `GET /activities` - returns activities dictionary
  - `POST /activities/{activity_name}/signup?email=...` - adds email to participants

## Project-Specific Patterns

### Data Access Pattern
```python
# Activities keyed by name, not ID
activities["Chess Club"]["participants"].append(email)
```

### Frontend API Integration
```javascript
// URL encoding for activity names with spaces
fetch(`/activities/${encodeURIComponent(activity)}/signup?email=${encodeURIComponent(email)}`)
```

### Error Handling
- **Backend**: FastAPI HTTPException with 404 for missing activities
- **Frontend**: Try/catch with user-friendly error messages, auto-hide after 5 seconds

## Common Tasks

### Adding New Activities
Modify the `activities` dictionary in `src/app.py` with the required structure:
```python
"Activity Name": {
    "description": "...",
    "schedule": "...", 
    "max_participants": number,
    "participants": []
}
```

### Frontend Updates
- Activity cards auto-generate from API data in `fetchActivities()`
- Form validation handled by HTML5 attributes and JavaScript
- Messages use CSS classes: `.success`, `.error`, `.hidden`

### Debugging
- Use VS Code debugger configuration for step-through debugging
- Frontend errors logged to browser console
- Backend logs visible in uvicorn terminal output

## External Dependencies
- **FastAPI**: Web framework with automatic OpenAPI docs
- **Uvicorn**: ASGI server for running FastAPI
- **No database**: Uses in-memory Python dictionaries
- **No build tools**: Static assets served directly by FastAPI