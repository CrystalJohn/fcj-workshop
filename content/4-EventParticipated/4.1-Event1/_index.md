---
title: "Event 1"
date: "2025-1205"
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Report: “BUILDING AGENTIC AI”

### Event Purpose

- Optimize context handling with Amazon Bedrock
- Build AI agent automation using Amazon Bedrock through practical techniques and real-world use cases

### Speakers

- **Nguyen Gia Hung** — Head of Solutions Architect, AWS
- **Kien Nguyen** — Solutions Architect, AWS
- **Viet Pham** — Founder & CEO, Diagflow
- **Kha Van** — Community Leader, AWS
- **Thang Ton** — Co-Founder & COO, Cloud Thinker
- **Henry Bui** — Head of Engineering, Cloud Thinker

### Key Content

#### Cost and performance optimization techniques for AI agent systems

- Long product release cycles → lost revenue / missed opportunities
- Inefficient operations → lower productivity, higher cost
- Non-compliance with security regulations → security incidents, reputational loss

#### Four "Quick Win" techniques for optimization

#### Prompt Caching
This was highlighted as the most important technique — it can reduce costs by 70–90% and speed up processing.
Context structure: split the context window into three parts: (1) System & Tool Schema, (2) Conversation History, and (3) Objective Prompt.
Common mistake: many teams only cache the System Prompt and Tool Schema, but Conversation History is often the most expensive part (80–90% of cost) and is neglected.
Correct strategy: place checkpoints so the entire conversation history can be cached. The first run (cache write) may add ~25% extra cost, but subsequent runs save ~90%.

#### Context Compaction (Summarization)

Cloud Thinker presented an intelligent summarization pattern to avoid losing cache hits.
Old approach: create a new agent to summarize past conversation — this invalidates the previous cache and often degrades quality.
New approach (Cloud Thinker technique): keep the same agent and cached history, but swap the Objective Prompt to a task like "summarize this conversation." This leverages cache hits, reduces summarization cost from ~$0.30 to ~$0.03 (≈90% savings) and improves output quality.

#### Tool Consolidation

Problems with protocols like MCP (Model Context Protocol) arise when too many tools (e.g., 50 tools) are injected into context, causing context flooding.
Solution: instead of embedding full complex schemas in the prompt, use a lightweight dictionary and consolidate instructions.
Just-in-time instruction: agents can request detailed tool usage instructions only when needed (e.g., via a `get instruction` call), reducing tokens sent in each request.

#### Parallel Tool Calling

Modern models allow calling multiple tools in parallel (instead of the older sequential ReAct pattern), saving time. This often requires explicit instructions (e.g., "maximize efficiency") to force parallel execution.

### What I Learned

#### Cost management strategies

- Input cost is a major part of operating AI agents in loops because each loop may re-send conversation history and system prompts. Longer histories increase input tokens and cost.
- Solution: use Prompt Caching and checkpointing to cut costs by up to 80–90%.

#### Smart Summarization techniques

- Keep the same agent and cached history to preserve cache hits and quality.
- This approach reduced summarization cost from $0.30 to $0.03 in practice and produced better outputs.

#### Tool Design: Avoid context flooding

- Problem: injecting too many tools (e.g., MCP with 50 tools) floods the context.
- Fix: provide a special instruction for agents to fetch detailed tool instructions on demand instead of embedding full schemas.
- Benefits: smaller context, lower token usage, better efficiency.

### Performance optimization: enforce Parallel Tool Calling

- Add explicit instructions to prompts so the model runs tasks in parallel and maximizes efficiency.

### Event Experience

Attending the **"Building Agentic AI"** workshop was an engaging technical experience that broadened my understanding of agent design and prompt engineering. Notable experiences included:

#### Learning from expert speakers
- Speakers from AWS, Cloud Thinker, and Diagflow shared practical best practices for designing modern agent-based applications.

#### Hands-on technical exercises
- Participated in a CloudThinker hackathon focused on optimizing systems for cost and performance using CloudThinker tools.

#### Exploring modern tools
- Hands-on exposure to CloudThinker and practical demonstrations of how to apply these techniques.

#### Event photos
![Event photo](/images/4-EventParticipated/5.12-event.jpg)

> Overall, the event not only delivered technical knowledge but also shifted my thinking about application design, agent workflows, and prompt optimization to achieve better results.
