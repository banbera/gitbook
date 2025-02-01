# Vision

Aesoperator is an open-source platform that lets AI agents fully operate computers, not just code, but perform any task a human user can. It leverages vision models (like Claude) to see interfaces through screenshots, persistent memory (via pgvector + Neon) to track context, and function calling for complex tasks. Currently browser-based with Firefox on Ubuntu 22.04, it will expand to a full desktop app by Q2 2025, enabling deeper system-level control.

• AI sees your interface, clicks buttons, fills forms, and navigates as if it were a human.\
• Persistent memory keeps track of tasks, data, and context across sessions.\
• It can call functions (serverless or local) to chain together powerful workflows.\
• Future expansions: more robust offline/desktop control, deeper integration with advanced LLMs, and advanced serverless deployments.

The goal is to create a universal “computer usage” layer so any language model can interact with any OS or application. This will include being able to operate your wallets, trade, setup notifications, everything you manually have to do and would take up tedious setups to get all right in tandem

## Core Capabilities

* Universal Computer Access: ✓ (Uses Firefox + system tools currently)
* Natural Interaction: ✓ (Claude for Vision so far)
* Memory & Context: (In beta development, Uses pgvector + Neon for RAG)
* Function Composition: (In progress Via Python SDK and task system)

## Key differentiators

* We are a platform-layer application that is built upon existing LLMs like Claude, Deepseek, and other language models that are:&#x20;
  * Multimodal
  * Function calling capabilities
* Same class of platform-layer as Devin(a computer use agent), OpenAI Operator, and Claude's hosted version of Computer Use
* Vision-First: ✓ (Core vision-based interaction model via Claude)
* Contextual Understanding: ✓ (Through RAG + persistent memory via pgvector)
* Serverless Architecture:  (Though transitioning to local desktop app in Q2)
* Open Protocol: ✓ (MCP implementation for tool/agent communication)

## Technical core

* Vision models: ✓ (Claude)
* LLMs:  (Claude, but DeepSeek R1-Zero in the future)
* Memory: ✓ (pgvector + Neon)
* Serverless:  (In progress)
* Browser/System: ✓ (Firefox + Ubuntu 22.04 tools)

The only nuance is that some features (like full system access) will be more robust in the desktop version coming in Q2 2025.

## Target Use Cases

1. **Research & Analysis**
   * Gathering data from multiple sources
   * Processing and synthesizing information
   * Generating comprehensive reports
2. **Workflow Automation**
   * Complex multi-step processes
   * Cross-application workflows
   * Data entry and validation
3. **System Administration**
   * Monitoring and maintenance
   * Resource management
   * Issue diagnosis and resolution
4. **Development & Testing**
   * UI testing automation
   * Bug reproduction
   * Development environment setup

## Future Roadmap

1. **Q1 2025**
   * O3-mini high and DeepSeek R1-Zero integration
   * Aquisition of an existing project and team
   * API access to Aesoperator
2. **Q2 2025**
   * Cross-platform support(Windows, MacOS, Linux)
   * Real-time collaboration
   * Aesoperator comes to your desktop
3. **Q3 2025**
   * Custom operator development
   * Enterprise integration
   * Advanced monitoring and analytics
