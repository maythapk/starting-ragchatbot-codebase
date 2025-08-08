# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Setup and Installation

```bash
# Install dependencies using uv package manager
uv sync

# Add new dependencies
uv add pakcage

# Set up environment variables (create .env file)
# Required: ANTHROPIC_API_KEY=your_anthropic_api_key_here
```


### Running the Application

```bash
# Quick start using the provided script
chmod +x run.sh
./run.sh

# Or manually start the backend server
cd backend
uv run uvicorn app:app --reload --port 8000
```

The application will be available at:
- Web Interface: http://localhost:8000
- API Documentation: http://localhost:8000/docs

## Project Guidance

- Always use uv to run the server and do not use pip directly
- Make sure to use uv to manage all dependencies

## Architecture Overview

This repository contains a Retrieval-Augmented Generation (RAG) system designed to answer questions about course materials using semantic search and AI-powered responses.

### Key Components

1. **RAG System Core** (`backend/rag_system.py`)
   - Main orchestrator that coordinates all other components
   - Handles document processing, vector storage, and AI generation
   - Manages conversation sessions and search tools

2. **Vector Storage** (`backend/vector_store.py`)
   - Uses ChromaDB for storing and retrieving document embeddings
   - Contains two collections:
     - Course catalog: Stores metadata about courses
     - Course content: Stores chunked content with embeddings

3. **Document Processing** (`backend/document_processor.py`)
   - Processes course documents and extracts structured information
   - Chunks text into semantic units for vector storage
   - Handles metadata extraction from documents

4. **AI Generator** (`backend/ai_generator.py`)
   - Interfaces with Anthropic's Claude API
   - Handles tool-based search and response generation
   - Manages conversation context for follow-up queries

5. **Search Tools** (`backend/search_tools.py`)
   - Defines tools for semantic search functionality
   - Formats search results for presentation
   - Tracks sources for attribution

6. **Session Management** (`backend/session_manager.py`)
   - Maintains conversation history with configurable limits
   - Creates and manages user sessions

7. **API Layer** (`backend/app.py`)
   - FastAPI application that exposes endpoints for:
     - Query processing (`/api/query`)
     - Course statistics (`/api/courses`)
   - Serves static frontend files

8. **Frontend** (`frontend/`)
   - Simple web interface with chat functionality
   - Displays course statistics
   - Handles conversation session persistence

### Data Flow

1. User query is sent from frontend to `/api/query` endpoint
2. RAG system retrieves conversation history for context
3. AI generator prepares a prompt with query and history
4. Search tool is executed to find relevant course content
5. Vector store performs semantic search in ChromaDB
6. Search results return to AI generator for response synthesis
7. Final response with sources is sent back to frontend
8. Conversation history is updated for future queries

### Document Format

Course documents in the `/docs` directory follow this structure:
- Line 1: `Course Title: [title]`
- Line 2: `Course Link: [url]`
- Line 3: `Course Instructor: [instructor]`
- Following lines: Lesson markers (e.g., `Lesson 0: Introduction`) and content

The system processes these documents, extracts metadata, and chunks content for semantic search.