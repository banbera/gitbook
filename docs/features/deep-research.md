# Deep research

Aesoperator's deep research capability performs comprehensive research by combining browser automation, data extraction, and memory-powered analysis. Research tasks typically take 2-8 hours depending on depth and scope.

## How It Works

The research process:

1. Takes a research topic/question and constraints as input
2. Uses Firefox to navigate and scrape relevant sources
3. Extracts and processes information using vision and language models
4. Builds a knowledge graph in pgvector for semantic search
5. Generates insights using LLM analysis

For example, researching "Latest advances in fusion energy":

1. Crawls scientific papers, news articles, and research lab websites
   * Uses Firefox with Selenium for web navigation
   * Accesses arXiv, Google Scholar, ScienceDirect via APIs
   * Downloads PDFs and HTML content for processing
2. Extracts key findings about recent breakthroughs and technical progress
   * Uses newsonnet Vision to analyze diagrams and figures
   * Uses newsonnet Vision to do any computer vision tasks
3. Builds knowledge graph connecting research teams, technologies, and results
   * Stores in PostgreSQL with pgvector extension
   * Uses Neo4j for graph relationships
   * Employs sentence transformers for semantic embeddings
4. Generates comprehensive report with:
   * Timeline of major developments
   * Technical analysis of competing approaches
   * Assessment of commercial viability
   * Future research directions

Similar to Devin's Notepad, Aesoperator is prompted to open Notes in Ubuntu to write a plan before completing them one by one such that if it fails at some portion of the workflow, it can self heal automatically
