# Requirements Document - CodeSwitch AI

## Project Overview
CodeSwitch AI is a web application that analyzes GitHub repositories and generates beginner-friendly learning roadmaps using AI. It helps developers understand repository structure, architecture, and provides guided learning paths.

## Functional Requirements

### FR1: Repository Analysis
- The system shall accept GitHub repository URLs as input
- The system shall parse GitHub URLs to extract owner and repository name
- The system shall fetch repository README content via GitHub API
- The system shall retrieve top-level repository structure (file/folder names)

### FR2: AI-Powered Analysis
- The system shall use OpenAI GPT-4o-mini to analyze repository information
- The system shall generate structured analysis including:
  - Summary of the repository
  - Architecture overview
  - Component breakdown
  - Technology stack identification
  - Learning roadmap for beginners
  - Contribution guide
- Analysis output shall be in JSON format

### FR3: User Interface
- The system shall provide a web-based interface for repository URL input
- The system shall display analysis results in a readable format
- The interface shall have a space-themed visual design
- The system shall provide real-time feedback during analysis

### FR4: API Integration
- The system shall integrate with GitHub API for repository data
- The system shall integrate with OpenAI API for AI analysis
- The system shall handle API authentication securely

## Non-Functional Requirements

### NFR1: Performance
- Repository analysis shall complete within 30 seconds under normal conditions
- The system shall handle concurrent requests efficiently

### NFR2: Security
- API keys shall be stored in environment variables
- The system shall not expose sensitive credentials in code
- CORS shall be configured to allow cross-origin requests

### NFR3: Usability
- The interface shall be intuitive and require minimal user training
- Error messages shall be clear and actionable
- The system shall work on modern web browsers

### NFR4: Reliability
- The system shall handle GitHub API rate limits gracefully
- The system shall provide meaningful error messages when APIs fail
- The system shall validate input URLs before processing

### NFR5: Maintainability
- Code shall follow Python and JavaScript best practices
- The system shall use modular architecture for easy updates
- Dependencies shall be clearly documented

## Technical Requirements

### TR1: Backend
- Python 3.8+
- FastAPI framework for REST API
- OpenAI Python SDK
- Requests library for HTTP calls
- Python-dotenv for environment management

### TR2: Frontend
- HTML5, CSS3, JavaScript (vanilla)
- Responsive design
- Fetch API for backend communication

### TR3: APIs
- GitHub REST API v3
- OpenAI Chat Completions API (GPT-4o-mini)

### TR4: Environment
- Backend server running on localhost:8000
- Environment variables for API keys:
  - OPENAI_API_KEY
  - GITHUB_TOKEN

## Constraints
- Requires active internet connection
- Requires valid OpenAI API key with available credits
- Subject to GitHub API rate limits (60 requests/hour unauthenticated, 5000/hour authenticated)
- Limited to public GitHub repositories or repositories accessible with provided token

## Future Enhancements
- Support for private repositories
- Caching mechanism for analyzed repositories
- Export analysis to PDF/Markdown
- Support for multiple AI models
- User authentication and history tracking
- Deep repository analysis (beyond top-level structure)
- Interactive learning path with progress tracking
