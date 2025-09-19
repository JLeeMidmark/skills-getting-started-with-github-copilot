# Mergington High School Activities API - Copilot Instructions

## Project Overview

This is a FastAPI-based web application for managing extracurricular activities at Mergington High School. Students can view available activities and sign up through both a web interface and REST API endpoints.

## Architecture & Key Components

**Backend**: Single FastAPI application (`src/app.py`) serving both API endpoints and static files
- **Data Storage**: In-memory Python dictionaries (resets on server restart)
- **API Pattern**: RESTful endpoints with activity names as identifiers
- **Static Files**: HTML/CSS/JS served from `/static` mount point

**Frontend**: Vanilla JavaScript SPA in `src/static/`
- `index.html`: Main page structure
- `app.js`: Handles API calls and DOM manipulation
- `styles.css`: Responsive styling with flexbox layout

## Development Workflows

### Quick Start
```bash
pip install -r requirements.txt
uvicorn src.app:app --reload
```
Access at: http://localhost:8000 (redirects to static site)
API docs: http://localhost:8000/docs

### File Structure
```
src/
├── app.py              # Main FastAPI application
├── README.md           # API documentation
└── static/
    ├── index.html      # Main web page
    ├── app.js          # Frontend logic
    └── styles.css      # Styling
.devcontainer/          # VS Code dev container setup
.github/workflows/      # GitHub Actions (exercise steps)
requirements.txt        # Python dependencies (fastapi, uvicorn)
```

## Project-Specific Patterns

### Data Model Conventions
- **Activities**: Use activity name as key (e.g., "Chess Club", "Programming Class")
- **Students**: Identified by email (format: `name@mergington.edu`)
- **Activity Structure**: `{description, schedule, max_participants, participants[]}`

### API Patterns
- `GET /activities`: Returns full activities dictionary
- `POST /activities/{activity_name}/signup?email=student@email`: Adds student to activity
- Root `/` redirects to static HTML page
- No authentication, validation, or data persistence

### Frontend Patterns
- **Async/await** for all API calls with try/catch error handling
- **DOM manipulation**: Dynamically builds activity cards and select options
- **Form handling**: Prevents default submission, uses fetch API
- **User feedback**: Shows success/error messages with auto-hide after 5 seconds

## Key Integration Points

### Static File Serving
```python
app.mount("/static", StaticFiles(directory=os.path.join(Path(__file__).parent, "static")), name="static")
```

### Frontend-Backend Communication
- JavaScript fetches from `/activities` endpoint
- Form submissions POST to `/activities/{name}/signup` with query parameter
- Error handling displays API error messages to users

## Development Environment

- **Container**: Python 3.13 dev container with auto port forwarding (8000)
- **Extensions**: GitHub Copilot, Python, debugpy pre-installed
- **Auto-setup**: Dependencies install on container creation

## Common Development Tasks

### Adding New Activities
Modify the `activities` dictionary in `src/app.py`:
```python
activities["New Activity"] = {
    "description": "Activity description",
    "schedule": "When it meets",
    "max_participants": 15,
    "participants": []
}
```

### Modifying API Behavior
- All endpoints are in `src/app.py`
- Use `HTTPException` for error responses
- Return JSON responses for API consistency

### Frontend Changes
- Modify `src/static/app.js` for new functionality
- Update `src/static/styles.css` for styling
- HTML structure in `src/static/index.html`

## Testing & Validation

```bash
# Test API endpoints
curl http://localhost:8000/activities
curl -X POST "http://localhost:8000/activities/Chess%20Club/signup?email=test@mergington.edu"

# Verify static files
curl http://localhost:8000/static/index.html
```

No formal test suite exists - validate changes through manual testing and API documentation at `/docs`.