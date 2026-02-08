# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an **educational workshop repository** for teaching LangGraph and LangChain concepts. It contains hands-on Jupyter notebooks that progressively teach agent-based AI development, from basic state and nodes to complex multi-agent workflows with human-in-the-loop patterns.

**Important**: This is a learning repository, not production code. The focus is on teaching concepts through interactive examples.

## Development Setup

### Prerequisites
- Python 3.11-3.13
- OpenAI API key (required)
- LangSmith API key (optional, for tracing)

### Environment Setup

```bash
# Navigate to the python directory
cd resources/lca-langgraph-essentials/python

# Copy environment template
cp example.env .env

# Edit .env and add your API keys:
# OPENAI_API_KEY=your_key_here
# LANGSMITH_API_KEY=your_key_here (optional)
# LANGSMITH_TRACING=true (optional)

# Install dependencies
uv sync

# Run Jupyter Lab
uv run jupyter lab

# Or activate virtual environment first
source .venv/bin/activate  # Windows: .venv\Scripts\activate
jupyter lab
```

### Optional: LangSmith Studio

```bash
# Copy .env to studio directory
cp .env ./studio/.

# Run LangGraph dev server
langgraph dev
```

### Code Quality

```bash
# Lint code
uv run ruff check

# Type check
uv run mypy .
```

## Repository Structure

The main course content is in `resources/lca-langgraph-essentials/python/`:

### Lab Sequence (L1-L6)

Each lab builds on previous concepts:

1. **L1_nodes.ipynb** - States & Nodes
   - Foundation: TypedDict state definitions
   - Nodes as Python functions
   - Basic linear graphs (START → Node → END)
   - Graph compilation and invocation

2. **L2_edges.ipynb** - Parallel Execution
   - State reducers (using `operator.add`)
   - Parallel node execution patterns
   - Graph branching and convergence
   - State merging from parallel paths

3. **L3-L4_cedges_memory.ipynb** - Conditional Edges & Memory
   - Dynamic node selection using `Command`
   - Alternative: `add_conditional_edges()` with routing functions
   - State persistence with `InMemorySaver`
   - Thread-based conversation management

4. **L5_interrupt.ipynb** - Human-in-the-Loop
   - Using `interrupt()` for workflow pauses
   - Human decision points in automated workflows
   - Resuming execution with human input

5. **L6_EmailAgent.ipynb** - Email Support Agent (Capstone)
   - Combines all learned concepts
   - Multi-node workflow: read → classify → search/bug_tracking → write → human_review → send
   - Parallel execution (search + bug tracking)
   - Conditional routing (auto-send vs human review)
   - Structured LLM outputs (type-safe classifications)
   - Urgency-based routing for high-priority emails

## LangGraph Architecture Patterns

### State-Based Workflow Model
- All data flows through a shared TypedDict state
- Nodes are stateless functions that transform state
- Reducers handle state merging from parallel paths (e.g., `operator.add` for lists)

### Graph Orchestration Patterns

**Linear Flow** (Lab 1):
```
START → Node A → Node B → END
```

**Parallel Execution** (Lab 2):
```
        ┌─→ Node A ─┐
START ──┤           ├─→ END
        └─→ Node B ─┘
```

**Conditional Routing** (Lab 3-4):
```
START → Router → ┬─→ Path A → END
                 ├─→ Path B → END
                 └─→ Path C → END
```

**Human-in-the-Loop** (Lab 5):
```
START → Process → [interrupt] → Human Review → Continue → END
```

### Key LangGraph Concepts

- **State**: TypedDict defining all workflow data
- **Nodes**: Functions that take state and return state updates
- **Edges**: Connections between nodes (normal, parallel, conditional)
- **Reducers**: Functions that merge state from parallel paths (e.g., `Annotated[list, operator.add]`)
- **Command**: Object for dynamic routing (`Command(goto="next_node")`)
- **Memory**: State persistence across invocations (checkpointers)
- **Interrupts**: Pause points for human input or external approval

### Production Patterns (from Email Agent)

- **Parallel task execution** for independent operations
- **Structured LLM outputs** using TypedDict schemas for type safety
- **Conditional human review** based on content classification
- **State persistence** for multi-turn workflows
- **Error handling** with custom exceptions (e.g., SearchAPIError)
- **Urgency-based routing** (critical/high priority flagged for review)

## Key Dependencies

- **LangGraph** (>=1.0.0) - Core framework for graph-based workflows
- **LangChain** (>=1.0.0) - AI framework abstractions
- **LangChain-OpenAI** (>=1.0.0) - OpenAI integration
- **LangChain-Anthropic** (>=1.0.0) - Anthropic Claude integration
- **Jupyter** (>=1.0.0) - Interactive notebook environment
- **LangGraph CLI** (>=0.4.0) - Command-line tools and dev server

## Linting Configuration

Ruff is configured with:
- Google docstring convention
- pycodestyle, pyflakes, isort, pydocstyle checks
- Allows `print()` statements (educational context)
- Ignores docstring requirements for labs (D101, D103)
- Ignores line length limits (E501)
