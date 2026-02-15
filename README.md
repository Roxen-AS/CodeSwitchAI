# CodeSwitch AI

Turn any public GitHub repository into a structured learning roadmap.

## Overview

CodeSwitch AI is an AI-powered developer productivity tool that transforms complex GitHub repositories into structured, beginner-friendly learning guides.

Open-source projects are powerful but often difficult to navigate due to unclear architecture, incomplete documentation, and lack of onboarding structure. CodeSwitch AI reduces this cognitive overload by analyzing repository metadata and generating an organized breakdown that accelerates understanding and contribution.

---

## Problem

Developers often struggle with:

- Understanding unfamiliar codebases  
- Interpreting system architecture  
- Identifying key modules  
- Knowing where to start contributing  

This results in high onboarding time and reduced participation in open-source development.

---

## Solution

CodeSwitch AI analyzes a public GitHub repository by extracting:

- README content  
- Top-level file and folder structure  

Using a large language model, it generates:

- Project summary  
- Architecture overview  
- Component breakdown  
- Technology stack identification  
- Step-by-step learning roadmap  
- Contribution starter guide  

The output is structured and organized to help developers move from exploration to contribution efficiently.

---

## Architecture

CodeSwitch AI follows a client–server architecture.

### Frontend
- HTML
- CSS
- JavaScript
- Collects repository URL
- Displays structured analysis

### Backend
- Python
- FastAPI
- Parses repository URL
- Fetches repository metadata via GitHub REST API
- Constructs structured AI prompt
- Calls OpenAI language model
- Returns structured JSON response

### External Services
- GitHub REST API
- OpenAI GPT-4o-mini

---

## Technologies Used

### Backend
- Python
- FastAPI
- Requests
- OpenAI SDK
- python-dotenv

### Frontend
- HTML
- CSS
- JavaScript

---

## Installation

Clone the repository:

