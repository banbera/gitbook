---
description: >-
  Note as of 0.1.1 Aesoperator SDK is still experimental and in development,
  these are functions that are still experimental
---

# Core Concepts

Aesoperator is an AI agent that can use computers just like a human would - interacting with applications, processing data, and completing complex tasks across your entire system. Here are the key concepts:

## Task

A Task represents any computer-based job you want Aesoperator to complete. This could be:

* Analyzing data across multiple applications
* Automating multi-step workflows
* Monitoring systems and responding to events
* Processing and organizing files
* Interacting with web applications

Note: The default configuration runs on Ubuntu 22.04, so Windows-only applications and features will not be available. Make sure your tasks are compatible with Linux environments.

Tasks are defined declaratively:

```python
task = aesop.Task(
    name="analyze_sales_data",
    description="Process quarterly sales reports and generate summary",
    inputs={
        "data_source": "sales_q4_2024.xlsx",
        "output_format": "powerpoint",
        "metrics": ["revenue", "growth", "forecasts"]
    },
    system_access=["excel", "powerpoint", "local_files"]
)
```

## System Access

Aesoperator can interact with its own Ubuntu 22.04 environment:

* **Applications**: Firefox, command line tools
* **Files**: Read, write, organize files and directories
* **System**: Monitor resources, run commands, manage processes
* **Network**: Make API calls, handle web requests, manage connections via the terminal
* **Data**: Process various file formats, query databases, transform data with LibreOffice

Example of system-wide access:

```python
# Work with LibreOffice
libreoffice_data = aesop.open_application("libreoffice", "sales_data.xlsx")
processed_data = libreoffice_data.run_analysis("quarterly_summary")

# Save to shared drive
aesop.system.copy_file("report.pptx", "//shared/reports/q4_2024/")

# Send notification
aesop.system.send_notification("Q4 report ready for review")
```

## Memory & Context

Aesoperator maintains context across your entire system:

### In the backend, Aesoperator maintains a persistent key-value store for its context:

```python
# Remember important file locations
aesop.memory.set("report_templates", "//shared/templates/")
aesop.memory.set("data_sources", {
    "sales": "//data/sales/",
    "inventory": "//data/inventory/"
})

# Track application states
aesop.memory.set("excel_instances", [
    {"file": "sales.xlsx", "sheet": "Q4", "last_cell": "B15"},
    {"file": "inventory.xlsx", "sheet": "Current", "filter": "Category=Electronics"}
])

# Maintain workflow progress
aesop.memory.set("analysis_state", {
    "steps_completed": ["data_import", "cleaning"],
    "current_step": "forecasting",
    "remaining": ["visualization", "export"]
})
```

## Actions

Actions represent discrete operations Aesoperator can perform:

### System Actions

```python
# File operations
aesop.system.copy_file(source, destination)
aesop.system.create_directory(path)
aesop.system.search_files(pattern)

# Process management
aesop.system.start_process(name)
aesop.system.monitor_resources()
aesop.system.kill_process(pid)

# Network operations
aesop.system.check_connection()
aesop.system.download_file(url)
```

### Application Actions

```python
# Native app control
app = aesop.open_application(name)
app.click_button("Save")
app.type_text("Hello world")
app.get_window_state()

# Browser automation *Planned feature*
browser = aesop.open_browser()
browser.navigate(url)
browser.fill_form(data)
```

### Data Actions

```python
# Data processing
data = aesop.read_file("data.csv")
transformed = aesop.transform_data(data, operations)
aesop.save_file("processed.csv", transformed)

# Database operations *Planned feature*
db = aesop.connect_database("postgres://...")
results = db.query("SELECT * FROM sales")
```

## Function Composition

Chain actions together for complex workflows:

**Note: This workflow is still really hard to get right as mistakes Aesoperator makes become exponentially more difficult to fix in-context and is a research problem.**

```python
def organize_research_papers(topic, max_papers=5):
    # Search and download papers task
    download_task = Task(
        name="download_papers",
        description=f"Find and download recent papers about {topic}",
        config=TaskConfig(
            max_pages=10,
            allowed_domains=["arxiv.org", "scholar.google.com"],
            max_duration_seconds=300
        ),
        inputs={
            "topic": topic,
            "max_results": max_papers,
            "date_range": "last_2_years"
        }
    )
    
    # Execute download task
    download_result = aesop.execute_task(download_task)
    downloaded_pdfs = download_result.files
    
    # Create organized folder structure
    folder_task = Task(
        name="organize_papers",
        description="Create folders and organize downloaded papers",
        inputs={
            "files": downloaded_pdfs,
            "base_path": f"~/research/{topic}",
            "organize_by": ["year", "author"]
        }
    )
    
    # Execute organization task
    folder_result = aesop.execute_task(folder_task)
    
    # Generate summary report
    summary_task = Task(
        name="summarize_papers",
        description="Create summary of downloaded papers",
        inputs={
            "pdf_files": downloaded_pdfs,
            "output_format": "markdown",
            "include_metadata": True
        }
    )
    
    summary_result = aesop.execute_task(summary_task)
    
    # Write summary to file
    aesop.system.write_file(
        f"~/research/{topic}/summary.md",
        summary_result.content
    )
    
    return {
        "papers": downloaded_pdfs,
        "summary": summary_result.content,
        "organized_path": folder_result.final_path
    }

# Use the composed workflow
result = organize_research_papers("quantum computing", max_papers=3)
```
