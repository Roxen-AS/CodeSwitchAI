# Design Document - CodeSwitch AI

## System Architecture

### High-Level Architecture
```
┌─────────────┐         ┌─────────────┐         ┌──────────────┐
│   Browser   │ ◄─────► │   FastAPI   │ ◄─────► │  GitHub API  │
│  (Frontend) │  HTTP   │  (Backend)  │  HTTPS  │              │
└─────────────┘         └─────────────┘         └──────────────┘
                              │
                              │ HTTPS
                              ▼
                        ┌──────────────┐
                        │  OpenAI API  │
                        └──────────────┘
```

### Architecture Pattern
- Client-Server Architecture
- RESTful API design
- Separation of concerns (Frontend/Backend)

## Component Design

### Backend Components

#### 1. main.py - API Server
- Framework: FastAPI
- Responsibilities:
  - HTTP request handling
  - CORS middleware configuration
  - Request/response validation
  - Orchestration of analysis workflow

- Endpoints:
  - `POST /analyze`: Accepts GitHub URL, returns AI analysis

#### 2. github_utils.py - GitHub Integration
- Functions:
  - `parse_github_url(url)`: Extracts owner and repo from URL
  - `get_readme(owner, repo)`: Fetches README content
  - `get_repo_structure(owner, repo)`: Retrieves top-level file structure

- Error Handling:
  - Returns empty string/list on API failures
  - Supports optional GitHub token authentication

#### 3. ai_engine.py - AI Analysis
- Functions:
  - `analyze_repository(readme, structure)`: Generates structured analysis

- AI Configuration:
  - Model: GPT-4o-mini
  - Temperature: 0.3 (for consistent, focused responses)
  - System prompt: Expert software mentor persona

- Output Format: JSON with keys:
  - summary
  - architecture
  - components
  - tech_stack
  - learning_roadmap
  - contribution_guide

#### 4. config.py - Configuration Management
- Loads environment variables using python-dotenv
- Manages API keys securely
- Centralized configuration access

### Frontend Components

#### 1. index.html - Structure
- Single-page application
- Input field for GitHub URL
- Result display area
- Space-themed design elements

#### 2. script.js - Logic
- `analyze()`: Sends POST request to backend
- Handles user interaction
- Updates UI with analysis results

#### 3. style.css - Presentation
- Dark theme with white accents
- Animated star field background
- Responsive input and button styling
- Monospace font for technical aesthetic

## Data Flow

### Analysis Request Flow
1. User enters GitHub URL in frontend
2. Frontend sends POST request to `/analyze` endpoint
3. Backend parses URL to extract owner/repo
4. Backend fetches README from GitHub API
5. Backend fetches repository structure from GitHub API
6. Backend sends README + structure to OpenAI API
7. OpenAI returns structured analysis
8. Backend returns analysis to frontend
9. Frontend displays results to user

### Data Models

#### Request Model (Pydantic)
```python
class RepoRequest(BaseModel):
    url: str
```

#### Response Model
```json
{
  "analysis": "JSON string containing structured analysis"
}
```

## API Design

### Backend API

#### POST /analyze
- Request Body:
  ```json
  {
    "url": "https://github.com/owner/repo"
  }
  ```

- Response:
  ```json
  {
    "analysis": "{...structured JSON analysis...}"
  }
  ```

- Status Codes:
  - 200: Success
  - 422: Validation error
  - 500: Server error

### External APIs

#### GitHub API
- Endpoint: `GET /repos/{owner}/{repo}/readme`
- Endpoint: `GET /repos/{owner}/{repo}/contents`
- Authentication: Bearer token (optional)

#### OpenAI API
- Endpoint: Chat Completions
- Model: gpt-4o-mini
- Authentication: API key

## Security Design

### API Key Management
- Keys stored in `.env` file (not committed to version control)
- Loaded via environment variables
- Never exposed in client-side code

### CORS Configuration
- Currently allows all origins (development mode)
- Should be restricted to specific domains in production

### Input Validation
- Pydantic models validate request structure
- URL parsing validates GitHub URL format

## UI/UX Design

### Visual Theme
- Space/sci-fi aesthetic
- Black background with white text
- Animated star field for visual interest
- Glowing text effects

### User Flow
1. User lands on homepage
2. User enters GitHub repository URL
3. User clicks "Analyze" button
4. System processes request (loading state implied)
5. Results displayed in formatted text area

### Responsive Design
- Input field: 60% width
- Result area: 70% width
- Centered layout
- Hover effects on interactive elements

## Error Handling Strategy

### Backend
- GitHub API failures: Return empty data, continue processing
- OpenAI API failures: Propagate error to frontend
- Invalid URLs: Handled by parsing logic

### Frontend
- Network errors: Displayed in result area
- Invalid responses: Shown as-is to user

## Deployment Considerations

### Development
- Backend: `uvicorn main:app --reload` on port 8000
- Frontend: Serve static files via local server or open directly

### Production Recommendations
- Use production ASGI server (Gunicorn + Uvicorn)
- Implement proper error handling and logging
- Add rate limiting
- Restrict CORS to specific domains
- Use HTTPS
- Implement caching for repeated repository analyses
- Add monitoring and analytics

## Technology Stack

### Backend
- Python 3.8+
- FastAPI (web framework)
- Uvicorn (ASGI server)
- OpenAI SDK
- Requests library
- Python-dotenv

### Frontend
- HTML5
- CSS3 (with animations)
- Vanilla JavaScript (ES6+)

### APIs
- GitHub REST API v3
- OpenAI Chat Completions API

## Design Patterns Used

1. MVC Pattern: Separation of concerns between frontend and backend
2. Repository Pattern: GitHub utilities abstract API interactions
3. Service Layer: AI engine encapsulates analysis logic
4. Configuration Pattern: Centralized config management
5. Middleware Pattern: CORS handling in FastAPI

## Performance Considerations

- Synchronous API calls (potential bottleneck)
- No caching mechanism (repeated analyses hit APIs)
- Single-threaded processing
- Network latency dependent on external APIs

### Optimization Opportunities
- Implement async/await for concurrent API calls
- Add Redis caching for repository data
- Implement request queuing for high load
- Add response streaming for large analyses
