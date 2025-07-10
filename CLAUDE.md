# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

SAGE (Socratic Agent for Gaming and Education) is a cognitive research platform focused on studying how AI systems can teach and learn from each other, with human oversight. The MVP uses checkers as the learning domain.

## Architecture Decision

The project has two PRD approaches documented:
1. **Teaching-focused** (`0_checkers/sage_checkers_python_prd.md`): AI teaching AI with human oversight
2. **Technical implementation** (`sage_checkers_python_prd.md`): Modern Python stack with CrewAI, Hugging Face, and Streamlit

**IMPORTANT**: Use the technical implementation PRD as the primary reference for development.

## Technology Stack

- **Language**: Python 3.9+
- **Agent Framework**: CrewAI for multi-agent orchestration
- **AI Models**: Hugging Face Transformers
- **UI Framework**: Streamlit
- **Database**: SQLite for session storage
- **Testing**: pytest
- **Linting**: black, flake8, mypy
- **Package Management**: pip with requirements.txt

## Common Development Commands

### Environment Setup
```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
source venv/bin/activate  # On macOS/Linux
# or
venv\Scripts\activate  # On Windows

# Install dependencies
pip install -r requirements.txt

# Install development dependencies
pip install -r requirements-dev.txt
```

### Running the Application
```bash
# Run the Streamlit app
streamlit run src/app.py

# Run with specific port
streamlit run src/app.py --server.port 8501

# Run in development mode with auto-reload
streamlit run src/app.py --server.runOnSave true
```

### Testing
```bash
# Run all tests
pytest

# Run tests with coverage
pytest --cov=src --cov-report=html

# Run specific test file
pytest tests/test_agents.py

# Run tests matching pattern
pytest -k "test_pattern_recognition"

# Run tests in verbose mode
pytest -v
```

### Code Quality
```bash
# Format code with black
black src/ tests/

# Check code style
flake8 src/ tests/

# Type checking
mypy src/

# Run all quality checks
make lint  # If Makefile is created
```

### Git Workflow
```bash
# Always create feature branches from main
git checkout -b feature/issue-number-description

# Before committing, run quality checks
black src/ tests/
flake8 src/ tests/
mypy src/
pytest

# Commit with descriptive messages referencing issues
git commit -m "feat: implement pattern recognition agent (#5)"

# Push and create PR
git push -u origin feature/issue-number-description
```

## Project Structure

```
go-planning/
├── src/
│   ├── app.py                    # Main Streamlit application
│   ├── agents/
│   │   ├── __init__.py
│   │   ├── pattern_agent.py      # Pattern recognition specialist
│   │   ├── strategy_agent.py     # Strategic planning director
│   │   ├── metacognitive_agent.py # Thinking process observer
│   │   └── social_agent.py       # Learning partnership facilitator
│   ├── game/
│   │   ├── __init__.py
│   │   ├── checkers.py           # Checkers game logic
│   │   └── board.py              # Board state management
│   ├── ui/
│   │   ├── __init__.py
│   │   ├── board_component.py    # Checkers board UI
│   │   ├── chat_interface.py     # Conversation interface
│   │   └── research_dashboard.py # Research analytics
│   ├── models/
│   │   ├── __init__.py
│   │   └── cognitive_models.py   # Hugging Face model management
│   └── research/
│       ├── __init__.py
│       ├── tracker.py            # Cognitive event tracking
│       └── analytics.py          # Research data analysis
├── tests/
│   ├── test_agents/
│   ├── test_game/
│   └── test_integration/
├── data/                         # SQLite databases and session data
├── notebooks/                    # Jupyter notebooks for research
├── requirements.txt              # Production dependencies
├── requirements-dev.txt          # Development dependencies
└── .streamlit/
    └── config.toml              # Streamlit configuration
```

## Key Architectural Patterns

### Multi-Agent Communication
The project uses CrewAI's hierarchical process for agent coordination. Each agent has specific roles and tools:

1. **Pattern Agent**: Identifies immediate tactical opportunities
2. **Strategy Agent**: Plans multi-move sequences
3. **Metacognitive Agent**: Explains reasoning processes
4. **Social Agent**: Manages teaching/learning interactions

### Three Cognitive Modes
1. **"Teach Me!"**: AI simulates being a learner
2. **"Let Me Teach You!"**: AI acts as a teacher
3. **"Let's Play!"**: Collaborative problem-solving

### Research Data Collection
All cognitive events (insights, breakthroughs, confusion moments) are tracked with timestamps and context for research analysis.

## GitHub Workflow

### Issue Types
- **feature**: New functionality
- **bug**: Something isn't working
- **research**: Cognitive research tasks
- **docs**: Documentation improvements
- **refactor**: Code improvements without changing functionality

### Pull Request Process
1. Create feature branch from main
2. Implement changes with tests
3. Run all quality checks
4. Create PR referencing issue(s)
5. Ensure CI passes
6. Request review
7. Merge after approval

### Commit Message Format
```
type: brief description (#issue-number)

Longer explanation if needed.

Co-Authored-By: Your Name <email@example.com>
```

Types: feat, fix, docs, refactor, test, chore

## Important Notes

1. **No Implementation Yet**: The repository is in planning phase. When implementing, create the directory structure first.

2. **Test-Driven Development**: Write tests before implementing features.

3. **Cognitive Focus**: Remember this is a research platform. All features should support understanding cognition.

4. **Data Privacy**: Never commit real user data or API keys.

5. **Performance**: The MVP doesn't require GPU. Optimize for CPU inference.

6. **Deployment**: Target Streamlit Cloud for easy deployment.

## Research Considerations

When implementing features, always consider:
- How does this help us understand thinking/learning?
- Can we measure cognitive development?
- Is the AI reasoning transparent to users?
- Are we collecting useful research data?

## Quick Start for New Features

1. Check existing issues or create new one
2. Create feature branch: `git checkout -b feature/issue-number-description`
3. Set up test file in `tests/`
4. Implement feature with tests
5. Update documentation if needed
6. Run quality checks: `black`, `flake8`, `mypy`, `pytest`
7. Create PR with clear description
8. Link PR to issue(s)