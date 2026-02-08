# Agentic AI Workshop Series

## Session 1: Foundations + Simple SQL Agent

### Concepts

- Agents vs traditional LLM apps — agents decide their own control flow
- The ReAct loop: Reason → Act → Observe → Repeat
- When you need an agent vs a simpler pipeline

### LangChain Essentials

- Core abstractions: models, prompts, tools, agent executor
- Hands-on: wire up a single tool agent

### Build: Simple SQL Agent

- Sample SQLite database setup
- SQL execution tool + system prompt with schema
- End-to-end: natural language → SQL → results → answer
- Discuss limitations (bad queries, hallucinated tables, ambiguity) — motivates later sessions

---

## Session 2: Tools, LangGraph Basics + Simple Multi-Agent

### Quick Recap of Session 1

- Agents vs traditional LLM apps — agents decide their own control flow
- The ReAct loop: Reason → Act → Observe → Repeat
- LangChain core abstractions: models, prompts, tools, agent executor

### Tool Definitions

- Anatomy of a tool: name, description, schema
- Why good naming and descriptions matter for agent reliability
- Schema design: typing parameters, required vs optional fields
- Common pitfalls: vague descriptions, missing edge cases

### LangGraph Basics

- Why LangGraph: orchestrating complex agent workflows
- Core concepts: state, nodes, edges
- Building your first graph: simple linear flow

### Build: Simple Routing Multi-Agent System

- Two specialised agents with a router
- Conditional edges: routing based on query type
- Shared state between agents

### Introducing Arize Phoenix

- Why observability matters: agents are non-deterministic and opaque
- Basic setup: install Phoenix, configure tracer
- Viewing traces from your multi-agent system

---
