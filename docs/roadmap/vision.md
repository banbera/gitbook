# Vision

Aesoperator is a platform for AI agents to interact with computers.

OpenAI Operator showed us last week that Computer Usage Agents are the next modality. We are building the open source version of this.



Just like Devin showed us that AI can be a fully capable software engineer and Cursor brought AI directly into your IDE, Aesoperator is bringing general computer usage capabilities to AI. Starting as a browser-based tool accessible through Telegram and Discord, we're transitioning to a full desktop application that lets AI agents control your entire computer through screenshots and natural language - imagine Devin's capabilities but for any computer task, not just coding. By acquiring and integrating with existing projects, we're building an open-source ecosystem that will let any AI system interact with computers naturally, just like a human would.



This is an extremely large vision but with the team, their connection, and funding from AESOP we can make this happen



[**Core Capabilities:**](#user-content-fn-1)[^1]

* Universal Computer Access: ✓ (Uses Firefox + system tools currently)
* Natural Interaction: ✓ (Uses O3-mini high for vision + DeepSeek R1-Zero for LLM)
* Memory & Context: ✓ (Uses pgvector + Neon for RAG)
* Function Composition: ✓ (Via Python SDK and task system)

**Key Differentiators for now:**

* Screenshot-First: ✓ (Core vision-based interaction model via Claude)
* Contextual Understanding: ✓ (Through RAG + persistent memory via pgvector)
* Serverless Architecture: ✓ (Though transitioning to local desktop app in Q2)
* Open Protocol: ✓ (MCP implementation for tool/agent communication)

**Technical Foundation:**

* Vision models: ✓ (Claude)
* LLMs:  (Claude, but DeepSeek R1-Zero in the future)
* Memory: ✓ (pgvector + Neon)
* Serverless:  (In progress)
* Browser/System: ✓ (Firefox + Ubuntu 22.04 tools)

The only nuance is that some features (like full system access) will be more robust in the desktop version coming in Q2 2024.

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

[^1]: 
