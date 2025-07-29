# Product Requirements Document: SAGE Cognitive Laboratory
## Python Implementation with Modern AI Agent Stack

### Executive Summary

**Product**: SAGE (Socratic Agent for Gaming and Education) - A cognitive research platform using checkers to study fundamental thinking processes, built with Python and established open-source AI agent frameworks.

**Technical Stack**: CrewAI + Hugging Face + Streamlit for rapid prototyping and deployment, leveraging battle-tested agent orchestration patterns.

**Core Innovation**: Multi-agent cognitive modeling using production-ready frameworks, making the research accessible to the broader Python AI community.

**Implementation Philosophy**: "Easy button" approach - use established patterns so the focus stays on cognitive research, not infrastructure debugging.

---

### Technical Architecture: Production-Ready Cognitive Agents

#### Core Stack Selection

**CrewAI for Agent Orchestration**
- Proven multi-agent framework with 100k+ developers
- Built-in role-playing and memory management
- Natural hierarchical task delegation
- Extensive community and documentation

**Hugging Face Ecosystem**
- Pre-trained models for immediate deployment
- Transformers library for custom cognitive architectures
- Datasets for training checkers-specific behaviors
- Model Hub for sharing cognitive agent variants

**Streamlit for Rapid UI Development**
- Real-time interface updates for cognitive state visualization
- Built-in session state management
- Easy deployment to Streamlit Cloud
- Component ecosystem for game boards and chat interfaces

#### Multi-Agent Cognitive Architecture

```python
from crewai import Agent, Task, Crew
from transformers import AutoModel, AutoTokenizer
import streamlit as st

# Four specialized cognitive agents
class CognitiveAgents:
    def __init__(self):
        self.pattern_agent = Agent(
            role="Pattern Recognition Specialist",
            goal="Identify board patterns and tactical opportunities",
            backstory="Expert at visual pattern matching and immediate threat assessment",
            tools=[board_analyzer, pattern_matcher],
            memory=True
        )
        
        self.strategy_agent = Agent(
            role="Strategic Planning Director", 
            goal="Develop multi-move plans and long-term positioning",
            backstory="Master of lookahead analysis and strategic goal-setting",
            tools=[minimax_engine, position_evaluator],
            memory=True
        )
        
        self.metacognitive_agent = Agent(
            role="Thinking Process Observer",
            goal="Monitor and explain reasoning processes transparently", 
            backstory="Cognitive scientist specializing in thinking about thinking",
            tools=[reasoning_tracer, explanation_generator],
            memory=True
        )
        
        self.social_agent = Agent(
            role="Learning Partnership Facilitator",
            goal="Manage teaching/learning interactions and theory of mind",
            backstory="Educational psychologist expert in adaptive pedagogy",
            tools=[learner_modeler, socratic_questioner],
            memory=True
        )
```

#### Hugging Face Model Integration

```python
from transformers import pipeline, AutoModel
import torch

class CognitiveModelManager:
    def __init__(self):
        # Pre-trained models for different cognitive functions
        self.explanation_model = pipeline(
            "text-generation",
            model="microsoft/DialoGPT-medium",
            device=0 if torch.cuda.is_available() else -1
        )
        
        self.board_vision_model = AutoModel.from_pretrained(
            "google/vit-base-patch16-224"  # Vision transformer for board state
        )
        
        self.strategic_reasoning_model = pipeline(
            "question-answering", 
            model="deepset/roberta-base-squad2"
        )
        
    def get_cognitive_explanation(self, game_state, thinking_process):
        """Generate natural language explanation of AI reasoning"""
        prompt = f"Current board position: {game_state}\nThinking: {thinking_process}\nExplanation:"
        return self.explanation_model(prompt, max_length=150, do_sample=True)[0]['generated_text']
```

---

### The Three Cognitive Modes

#### "Teach Me!" Mode - AI Learning Simulation

```python
class LearnerPersona:
    def __init__(self):
        self.confusion_model = pipeline("text-classification", 
                                      model="j-hartmann/emotion-english-distilroberta-base")
        self.skill_level = 0.1  # Starts genuinely confused
        self.learning_curve = ExponentialGrowth(rate=0.15)
        
    def generate_learner_response(self, human_explanation, board_state):
        # Simulate realistic learning progression
        understanding_level = self.learning_curve.current_level()
        
        if understanding_level < 0.3:
            return self.generate_confusion_question(human_explanation)
        elif understanding_level < 0.7:
            return self.generate_partial_understanding(human_explanation)
        else:
            return self.generate_insight_moment(human_explanation)
```

#### "Let Me Teach You!" Mode - AI Pedagogy

```python
class TeacherPersona:
    def __init__(self):
        self.pedagogy_crew = Crew(
            agents=[self.socratic_questioner, self.hint_provider, self.encourager],
            tasks=[assess_understanding, provide_guidance, celebrate_progress],
            process=Process.hierarchical
        )
        
    def adaptive_teaching_response(self, human_move, skill_assessment):
        # Use CrewAI to orchestrate teaching strategy
        teaching_task = Task(
            description=f"Human made move: {human_move}. Skill level: {skill_assessment}. Provide appropriate guidance.",
            agent=self.socratic_questioner,
            expected_output="Socratic question that guides without giving answer"
        )
        
        return self.pedagogy_crew.kickoff([teaching_task])
```

#### "Let's Play!" Mode - Collaborative Cognition

```python
class CollaborativePersona:
    def __init__(self):
        self.thinking_crew = Crew(
            agents=[pattern_agent, strategy_agent, metacognitive_agent],
            tasks=[analyze_position, plan_strategy, explain_reasoning],
            process=Process.sequential,
            memory=True,
            verbose=True  # Show all agent interactions
        )
        
    def transparent_move_planning(self, board_state):
        # All cognitive agents work together transparently
        analysis_task = Task(
            description=f"Analyze position: {board_state}",
            agent=self.pattern_agent
        )
        
        strategy_task = Task(
            description="Based on pattern analysis, develop strategic plan",
            agent=self.strategy_agent,
            context=[analysis_task]
        )
        
        explanation_task = Task(
            description="Explain the reasoning process to human partner",
            agent=self.metacognitive_agent,
            context=[analysis_task, strategy_task]
        )
        
        return self.thinking_crew.kickoff([analysis_task, strategy_task, explanation_task])
```

---

### Streamlit Interface: Cognitive Observatory

#### Main Interface Structure

```python
import streamlit as st
import chess_board_component  # Custom Streamlit component
from streamlit_chat import message

def main_interface():
    st.set_page_config(page_title="SAGE Cognitive Laboratory", layout="wide")
    
    col1, col2 = st.columns([3, 2])
    
    with col1:
        # Interactive checkers board
        board_state = chess_board_component.streamlit_board(
            key="checkers_board",
            board=st.session_state.game.board,
            on_move=handle_move,
            highlight_legal_moves=True
        )
    
    with col2:
        # Three-panel cognitive interface
        chat_tab, cognition_tab, research_tab = st.tabs(
            ["💬 Conversation", "🧠 AI Thinking", "📝 Research Notes"]
        )
        
        with chat_tab:
            render_conversation_interface()
            
        with cognition_tab:
            render_cognitive_state_visualization()
            
        with research_tab:
            render_research_notebook()

def render_cognitive_state_visualization():
    """Real-time visualization of AI cognitive processes"""
    
    # Pattern Recognition Confidence
    pattern_confidence = st.session_state.cognitive_state['pattern_confidence']
    st.metric("Pattern Recognition", f"{pattern_confidence:.2f}", delta=None)
    st.progress(pattern_confidence)
    
    # Strategic Depth Visualization
    strategic_depth = st.session_state.cognitive_state['strategic_depth']
    st.subheader("Strategic Analysis Depth")
    
    # Tree visualization of move analysis
    with st.expander("Move Analysis Tree"):
        st.json(st.session_state.cognitive_state['move_tree'])
    
    # Metacognitive Commentary
    st.subheader("AI Self-Reflection")
    metacognitive_text = st.session_state.cognitive_state['self_reflection']
    st.write(metacognitive_text)
    
    # Social Model (Theory of Mind)
    st.subheader("AI's Model of Human Player")
    human_model = st.session_state.cognitive_state['human_model']
    col1, col2 = st.columns(2)
    with col1:
        st.metric("Estimated Skill Level", human_model['skill_level'])
    with col2:
        st.metric("Learning Style", human_model['learning_preference'])
```

#### Conversation Interface with Cognitive Context

```python
def render_conversation_interface():
    """Chat interface with move-contextualized AI responses"""
    
    # Mode selection
    mode = st.radio(
        "Choose interaction mode:",
        ["🎓 Teach Me!", "👨‍🏫 Let Me Teach You!", "🤝 Let's Play Together!"],
        horizontal=True
    )
    
    # Chat history
    for msg in st.session_state.conversation_history:
        if msg['sender'] == 'human':
            message(msg['content'], is_user=True, key=f"user_{msg['id']}")
        else:
            # AI messages include cognitive context
            message(
                msg['content'], 
                is_user=False, 
                key=f"ai_{msg['id']}",
                avatar_style="bottts",  # AI avatar
            )
            
            # Show cognitive reasoning behind response
            with st.expander("🧠 AI Reasoning"):
                st.write(msg['reasoning'])
    
    # Input for new messages
    if prompt := st.chat_input("Type your message..."):
        handle_human_message(prompt, mode)

def handle_human_message(message, mode):
    """Process human input through appropriate cognitive agents"""
    
    # Generate AI response based on current mode
    if mode == "🎓 Teach Me!":
        response = st.session_state.learner_persona.respond(message, st.session_state.game_state)
    elif mode == "👨‍🏫 Let Me Teach You!":
        response = st.session_state.teacher_persona.respond(message, st.session_state.game_state)
    else:  # Collaborative mode
        response = st.session_state.collaborative_persona.respond(message, st.session_state.game_state)
    
    # Update conversation history with cognitive context
    st.session_state.conversation_history.append({
        'id': len(st.session_state.conversation_history),
        'sender': 'human',
        'content': message,
        'timestamp': datetime.now()
    })
    
    st.session_state.conversation_history.append({
        'id': len(st.session_state.conversation_history),
        'sender': 'ai',
        'content': response['message'],
        'reasoning': response['cognitive_process'],
        'timestamp': datetime.now()
    })
```

---

### Research Infrastructure and Data Collection

#### Cognitive Emergence Tracking

```python
from dataclasses import dataclass
from typing import List, Dict, Any
import pandas as pd
import plotly.express as px

@dataclass
class CognitiveEvent:
    timestamp: datetime
    event_type: str  # 'insight', 'confusion', 'breakthrough', 'teaching_moment'
    context: Dict[str, Any]
    user_self_report: str
    ai_cognitive_state: Dict[str, Any]

class CognitiveResearchTracker:
    def __init__(self):
        self.events: List[CognitiveEvent] = []
        self.session_data = SessionData()
        
    def track_thinking_depth_change(self, previous_depth, new_depth, trigger_move):
        """Track when strategic thinking deepens"""
        if new_depth > previous_depth + 1:  # Significant jump
            self.events.append(CognitiveEvent(
                timestamp=datetime.now(),
                event_type='strategic_breakthrough',
                context={'old_depth': previous_depth, 'new_depth': new_depth, 'trigger': trigger_move},
                user_self_report=self.prompt_user_reflection(),
                ai_cognitive_state=self.get_current_ai_state()
            ))
    
    def detect_aha_moment(self, conversation_analysis):
        """Detect genuine insight moments from conversation patterns"""
        if conversation_analysis['excitement_level'] > 0.8 and \
           conversation_analysis['question_to_statement_ratio'] < 0.3:
            self.events.append(CognitiveEvent(
                timestamp=datetime.now(),
                event_type='aha_moment',
                context=conversation_analysis,
                user_self_report=self.prompt_user_reflection("What just clicked for you?"),
                ai_cognitive_state=self.get_current_ai_state()
            ))
    
    def generate_research_insights(self):
        """Generate research dashboard with cognitive development patterns"""
        df = pd.DataFrame([event.__dict__ for event in self.events])
        
        # Visualizations for research analysis
        fig_timeline = px.timeline(df, x_start='timestamp', x_end='timestamp', 
                                 y='event_type', title='Cognitive Development Timeline')
        
        fig_depth = px.line(self.session_data.strategic_depth_over_time, 
                           title='Strategic Thinking Depth Development')
        
        return {
            'timeline': fig_timeline,
            'depth_progression': fig_depth,
            'insights_summary': self.analyze_cognitive_patterns(),
            'raw_data': df
        }
```

#### Research Notebook Interface

```python
def render_research_notebook():
    """Interface for users to record cognitive insights"""
    
    st.subheader("Your Cognitive Journey")
    
    # Self-reflection prompts
    with st.expander("💭 Reflection Questions"):
        reflection = st.text_area(
            "What did you notice about your thinking process this session?",
            key="reflection_input"
        )
        
        if st.button("Save Reflection"):
            st.session_state.research_tracker.add_reflection(reflection)
            st.success("Reflection saved!")
    
    # Cognitive development visualizations
    st.subheader("Your Thinking Development")
    research_data = st.session_state.research_tracker.generate_research_insights()
    
    st.plotly_chart(research_data['depth_progression'])
    st.plotly_chart(research_data['timeline'])
    
    # Download research data
    if st.button("Download Session Data"):
        csv = research_data['raw_data'].to_csv(index=False)
        st.download_button(
            label="Download CSV",
            data=csv,
            file_name=f"cognitive_session_{datetime.now().strftime('%Y%m%d_%H%M%S')}.csv",
            mime="text/csv"
        )
```

---

### Implementation Timeline: Easy Button Approach

#### Phase 1: Core Framework (2 weeks)
**Week 1**: Basic Streamlit app with checkers board and CrewAI agent setup
- Install and configure CrewAI, Transformers, Streamlit
- Create basic checkers game logic
- Implement simple agent communication

**Week 2**: Hugging Face model integration and basic conversation
- Integrate pre-trained models for explanation generation
- Basic chat interface with move-contextualized responses
- Simple cognitive state visualization

#### Phase 2: Cognitive Modes (3 weeks)
**Week 3**: "Let's Play!" collaborative mode
- Transparent AI reasoning display
- Multi-agent move analysis with CrewAI
- Real-time cognitive state updates

**Week 4**: "Teach Me!" learner simulation
- Believable learning progression modeling
- Question generation and confusion simulation
- Progress tracking and breakthrough detection

**Week 5**: "Let Me Teach You!" pedagogy mode
- Socratic questioning implementation
- Adaptive difficulty and hint systems
- Teaching effectiveness measurement

#### Phase 3: Research Infrastructure (2 weeks)
**Week 6**: Data collection and analysis tools
- Cognitive event tracking system
- Research dashboard with Plotly visualizations
- Session data export functionality

**Week 7**: Deployment and user testing
- Streamlit Cloud deployment
- User testing with cognitive research focus
- Iterative improvements based on feedback

---

### Deployment: One-Click Research Platform

#### Streamlit Cloud Deployment
```python
# requirements.txt
streamlit==1.28.0
crewai==0.1.55
transformers==4.35.0
torch==2.1.0
plotly==5.17.0
pandas==2.1.3
numpy==1.24.3
```

#### Environment Configuration
```python
# .streamlit/config.toml
[theme]
primaryColor = "#4A90E2"
backgroundColor = "#FFFFFF"
secondaryBackgroundColor = "#F0F2F6"
textColor = "#262730"

[server]
maxUploadSize = 200
enableCORS = false
```

#### Easy Deployment Script
```bash
#!/bin/bash
# deploy.sh - One command deployment
pip install -r requirements.txt
export HUGGINGFACE_TOKEN=$1
streamlit run cognitive_lab.py --server.port 8501
```

---

### Success Metrics: Research Validation

#### Primary Success Indicators
1. **Genuine Engagement**: Users spend 20+ minutes per session voluntarily
2. **Cognitive Insights**: Users report new understanding of their thinking processes
3. **Research Value**: Platform generates publishable insights about human cognition
4. **Technical Reliability**: 99%+ uptime with smooth AI agent interactions

#### Measurable Research Outcomes
- **Strategic Depth Development**: Track increasing lookahead depth over sessions
- **Teaching Effectiveness**: Measure improvement in AI learning mode responses
- **Metacognitive Awareness**: Quantify increase in thinking-about-thinking observations
- **Theory of Mind Growth**: Track improvements in modeling AI mental states

#### Platform Usage Analytics
```python
class UsageAnalytics:
    def track_session_metrics(self):
        return {
            'session_duration': self.get_session_duration(),
            'mode_preferences': self.get_mode_usage_distribution(),
            'cognitive_events_per_session': len(self.cognitive_events),
            'user_engagement_score': self.calculate_engagement_score(),
            'research_insights_generated': len(self.unique_insights)
        }
```

---

### Conclusion: Production-Ready Cognitive Research

This Python implementation leverages the mature AI agent ecosystem to create a robust cognitive research platform. By using CrewAI, Hugging Face, and Streamlit, we get:

- **Rapid Development**: Established frameworks mean focus on research, not infrastructure
- **Community Support**: Large Python AI community for troubleshooting and contributions
- **Scalable Architecture**: Production-ready components that can handle real user loads
- **Research Validity**: Professional-grade data collection and analysis capabilities

The "easy button" approach means researchers can deploy and start gathering cognitive insights immediately, while the multi-agent architecture provides the sophistication needed for genuine cognitive modeling.

Most importantly: the platform remains focused on the core mission of understanding how thinking, learning, and teaching actually work, using checkers as the accessible laboratory for these fundamental cognitive explorations.