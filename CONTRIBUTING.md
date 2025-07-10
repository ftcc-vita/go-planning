# Contributing to SAGE

Thank you for your interest in contributing to SAGE (Socratic Agent for Gaming and Education)! This document provides guidelines for contributing to the project.

## Table of Contents
- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Issue Management](#issue-management)
- [Pull Request Process](#pull-request-process)
- [Coding Standards](#coding-standards)
- [Testing Guidelines](#testing-guidelines)
- [Documentation](#documentation)
- [Research Contributions](#research-contributions)

## Code of Conduct

We are committed to providing a welcoming and inclusive environment. Please:
- Be respectful and considerate
- Welcome newcomers and help them get started
- Focus on constructive criticism
- Respect differing viewpoints and experiences

## Getting Started

1. **Fork the repository** on GitHub
2. **Clone your fork** locally:
   ```bash
   git clone https://github.com/YOUR-USERNAME/go-planning.git
   cd go-planning
   ```
3. **Add upstream remote**:
   ```bash
   git remote add upstream https://github.com/ftcc-vita/go-planning.git
   ```
4. **Set up development environment**:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   pip install -r requirements.txt
   pip install -r requirements-dev.txt
   ```

## Development Workflow

### 1. Start with an Issue
- Check existing issues or create a new one
- Comment on the issue to claim it
- Wait for confirmation before starting work

### 2. Create a Feature Branch
```bash
# Update your main branch
git checkout main
git pull upstream main

# Create and checkout feature branch
git checkout -b feature/issue-number-brief-description
```

Branch naming conventions:
- `feature/123-add-pattern-agent` - New features
- `fix/456-correct-board-display` - Bug fixes
- `docs/789-update-readme` - Documentation
- `research/234-cognitive-metrics` - Research tasks
- `refactor/567-improve-agent-comm` - Code improvements

### 3. Make Your Changes
- Write code following our [coding standards](#coding-standards)
- Add tests for new functionality
- Update documentation as needed
- Commit frequently with clear messages

### 4. Commit Messages
Follow the conventional commits format:
```
type(scope): brief description

Longer explanation if needed. Wrap at 72 characters.

Closes #123
```

Types:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, etc.)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

### 5. Push and Create PR
```bash
# Push to your fork
git push origin feature/issue-number-brief-description

# Create PR via GitHub UI
```

## Issue Management

### Creating Issues
Use the appropriate issue template:
- **Feature Request**: New functionality
- **Bug Report**: Something not working
- **Research Task**: Cognitive research objectives
- **Agent Implementation**: AI agent development
- **UI Component**: Streamlit interface work

### Issue Labels
- `good first issue` - Suitable for newcomers
- `help wanted` - Community contributions welcome
- `priority: high/medium/low` - Issue priority
- `blocked` - Waiting on dependencies
- `in progress` - Currently being worked on

### Claiming Issues
1. Comment on the issue expressing interest
2. Wait for assignment from maintainers
3. Ask questions if requirements unclear
4. Update progress regularly

## Pull Request Process

### Before Creating a PR
1. **Ensure all tests pass**:
   ```bash
   pytest
   ```

2. **Run code quality checks**:
   ```bash
   black src/ tests/
   flake8 src/ tests/
   mypy src/
   ```

3. **Update documentation** if needed

4. **Test manually** in Streamlit:
   ```bash
   streamlit run src/app.py
   ```

### PR Guidelines
- Reference the issue(s) being addressed
- Provide clear description of changes
- Include screenshots for UI changes
- Explain research implications if applicable
- Complete the PR template checklist

### Review Process
1. Automated checks must pass
2. At least one maintainer review required
3. Address review feedback promptly
4. Squash commits if requested
5. Maintain professional discussion

## Coding Standards

### Python Style
- Follow PEP 8
- Use `black` for formatting (line length 88)
- Add type hints for all functions
- Write descriptive variable names

### Code Organization
```python
# Import order
import standard_library_modules
import third_party_modules
import local_modules

# Class structure
class AgentName:
    """Brief description of agent purpose.
    
    Longer description if needed.
    
    Attributes:
        attribute_name: Description
    """
    
    def __init__(self):
        """Initialize the agent."""
        pass
    
    def public_method(self, param: Type) -> ReturnType:
        """Brief description of method.
        
        Args:
            param: Description of parameter
            
        Returns:
            Description of return value
        """
        pass
```

### Agent Development
When creating CrewAI agents:
```python
agent = Agent(
    role="Clear, specific role",
    goal="Measurable goal",
    backstory="Relevant background that shapes behavior",
    tools=[appropriate_tools],
    memory=True,  # Enable memory for learning
    verbose=True  # Enable for debugging
)
```

## Testing Guidelines

### Test Structure
```
tests/
├── unit/           # Unit tests for individual components
├── integration/    # Integration tests for agent interactions
├── e2e/           # End-to-end tests for user workflows
└── fixtures/      # Test data and utilities
```

### Writing Tests
```python
import pytest
from src.agents import PatternAgent

class TestPatternAgent:
    """Test suite for Pattern Recognition Agent."""
    
    @pytest.fixture
    def agent(self):
        """Create agent instance for testing."""
        return PatternAgent()
    
    def test_pattern_detection(self, agent):
        """Test that agent detects basic patterns."""
        board_state = create_test_board()
        patterns = agent.detect_patterns(board_state)
        assert len(patterns) > 0
        assert patterns[0].type == "defensive_formation"
```

### Test Requirements
- Minimum 80% code coverage for new code
- All public methods must have tests
- Test edge cases and error conditions
- Use meaningful test names

## Documentation

### Code Documentation
- All modules need docstrings
- All public functions need docstrings
- Complex logic needs inline comments
- Update README.md for user-facing changes

### Research Documentation
- Document cognitive hypotheses being tested
- Explain data collection methodology
- Include analysis of results
- Reference relevant research papers

### User Documentation
- Provide clear setup instructions
- Include usage examples
- Document all UI interactions
- Explain cognitive modes clearly

## Research Contributions

### Research-Focused Contributions
SAGE is a cognitive research platform. We welcome:
- New cognitive measurement approaches
- Analysis of learning patterns
- Teaching effectiveness studies
- Theory of mind implementations

### Data Collection Ethics
- Never collect personally identifiable information
- Anonymize all research data
- Respect user privacy
- Follow research ethics guidelines

### Publishing Research
If your contributions lead to research insights:
1. Discuss with maintainers
2. Collaborate on paper writing
3. Ensure proper attribution
4. Share findings with community

## Questions?

- Check existing documentation
- Search closed issues
- Ask in issue comments
- Join community discussions

Thank you for contributing to SAGE! Together we're advancing our understanding of cognition, learning, and teaching through AI research.