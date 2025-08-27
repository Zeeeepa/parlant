# Usage Schemes

This document outlines the usage schemes for the integrated Windows PC assistant that combines Archon, OxyGent, and parlant frameworks.

## 1. Core Usage Patterns

### 1.1 Basic User Interaction Flow

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│  User    │     │ Assistant│     │ Agent    │     │  Tool    │
│ Interface│     │ Core     │     │ Executor │     │ Executor │
└────┬─────┘     └────┬─────┘     └────┬─────┘     └────┬─────┘
     │                │                │                │
     │ User Request   │                │                │
     │───────────────>│                │                │
     │                │                │                │
     │                │ Select Agents  │                │
     │                │───────────────>│                │
     │                │                │                │
     │                │                │ Execute Tools  │
     │                │                │───────────────>│
     │                │                │                │
     │                │                │ Tool Results   │
     │                │                │<───────────────│
     │                │                │                │
     │                │ Agent Results  │                │
     │                │<───────────────│                │
     │                │                │                │
     │ Response       │                │                │
     │<───────────────│                │                │
     │                │                │                │
```

### 1.2 Command Line Usage

```bash
# Start the Windows PC Assistant
pc-assistant start

# Execute a specific command
pc-assistant execute "open Chrome and search for weather"

# Query knowledge base
pc-assistant query "how to use Windows 11 settings"

# Register a new tool
pc-assistant register-tool --name "custom_tool" --module "my_tools.py" --function "my_function"

# Create a new agent
pc-assistant create-agent --name "custom_agent" --config "agent_config.yaml"

# Stop the assistant
pc-assistant stop
```

### 1.3 API Usage

```python
# Import the assistant SDK
from pc_assistant import PCAssistant

# Initialize the assistant
assistant = PCAssistant(config_path="config.yaml")

# Start the assistant
assistant.start()

# Process a user request
response = assistant.process_request({
    "type": "command",
    "content": "open Chrome and search for weather",
    "metadata": {
        "source": "api",
        "timestamp": "2023-08-27T14:30:00Z"
    }
})

# Execute a specific tool
result = assistant.execute_tool("ui_click", {
    "x": 100,
    "y": 200,
    "button": "left"
})

# Query knowledge
knowledge = assistant.query_knowledge("how to use Windows 11 settings")

# Stop the assistant
assistant.stop()
```

## 2. Framework-Specific Usage

### 2.1 OxyGent Orchestration Usage

```python
# Import OxyGent components
from oxygent.oxy.agents import ChatAgent, ReactAgent, WorkflowAgent
from oxygent.oxy.flows import ParallelFlow, PlanAndSolve
from oxygent.oxy.base_flow import BaseFlow

# Create a custom flow
class CustomFlow(BaseFlow):
    def __init__(self, config=None):
        super().__init__(config or {})
        self.chat_agent = ChatAgent(config.get("chat", {}))
        self.react_agent = ReactAgent(config.get("react", {}))
        
    def execute(self, request, context=None):
        # Generate initial response with chat agent
        chat_response = self.chat_agent.chat(request)
        
        # Use react agent to perform actions
        react_result = self.react_agent.run(chat_response)
        
        return {
            "response": chat_response,
            "actions": react_result
        }

# Register and use the custom flow
flow_manager = assistant.get_orchestration_layer().flow_manager
flow_manager.register_flow("custom", CustomFlow)

# Execute the flow
result = flow_manager.execute_flow("custom", "open Chrome and search for weather")
```

### 2.2 parlant Compliance Usage

```python
# Import parlant components
from parlant.core.engines.alpha import Engine
from parlant.core.engines.alpha.guideline_matching import GuidlineMatcher

# Create a custom guideline
guideline = {
    "id": "browser_usage",
    "description": "Guidelines for browser usage",
    "rules": [
        {
            "id": "allowed_domains",
            "description": "Only allow specific domains",
            "domains": ["google.com", "github.com", "microsoft.com"]
        },
        {
            "id": "browser_preference",
            "description": "Prefer Chrome over other browsers",
            "preferred_browser": "chrome"
        }
    ]
}

# Register the guideline
compliance_engine = assistant.get_compliance_layer().engine
compliance_engine.register_guideline(guideline)

# Validate an action against guidelines
validation = compliance_engine.validate_action({
    "type": "browser_open",
    "browser": "firefox",
    "url": "https://example.com"
})

# Check validation results
if validation["compliant"]:
    print("Action complies with guidelines")
else:
    print("Action violates guidelines:")
    for violation in validation["violations"]:
        print(f"- {violation['rule']}: {violation['description']}")
```

### 2.3 Archon Knowledge Usage

```python
# Import Archon components
from archon.agents import RAGAgent, DocumentAgent
from archon.server.services.knowledge import KnowledgeManager

# Process and index a document
document_agent = assistant.get_knowledge_layer().document_processor
document_result = document_agent.process({
    "title": "Windows 11 Guide",
    "content": "Windows 11 is the latest operating system from Microsoft...",
    "url": "https://example.com/windows11-guide",
    "type": "text"
})

# Query knowledge using RAG
rag_agent = assistant.get_knowledge_layer().rag_engine
query_result = rag_agent.query("How do I customize the Start menu in Windows 11?")

# Get knowledge sources
knowledge_manager = assistant.get_knowledge_layer().knowledge_manager
sources = knowledge_manager.get_sources()

# Print available sources
for source in sources:
    print(f"Source: {source['title']}, Type: {source['type']}, Items: {source['item_count']}")
```

## 3. Integration Usage Patterns

### 3.1 Tool Integration Usage

```python
# Define a custom tool
def custom_screenshot_tool(params):
    """
    Take a screenshot of a specific region
    
    Parameters:
    - x: X coordinate of the top-left corner
    - y: Y coordinate of the top-left corner
    - width: Width of the region
    - height: Height of the region
    - filename: Output filename
    
    Returns:
    - path: Path to the saved screenshot
    - success: Whether the operation was successful
    """
    import pyautogui
    
    # Extract parameters
    x = params.get("x", 0)
    y = params.get("y", 0)
    width = params.get("width", 800)
    height = params.get("height", 600)
    filename = params.get("filename", "screenshot.png")
    
    # Take screenshot
    try:
        screenshot = pyautogui.screenshot(region=(x, y, width, height))
        screenshot.save(filename)
        return {
            "path": filename,
            "success": True
        }
    except Exception as e:
        return {
            "error": str(e),
            "success": False
        }

# Register the custom tool
tool_registry = assistant.get_tool_layer().mcp_registry
tool_registry.register_tool("custom_screenshot", custom_screenshot_tool)

# Use the custom tool
result = tool_registry.execute_tool("custom_screenshot", {
    "x": 100,
    "y": 100,
    "width": 400,
    "height": 300,
    "filename": "my_screenshot.png"
})

# Check the result
if result["success"]:
    print(f"Screenshot saved to {result['path']}")
else:
    print(f"Failed to take screenshot: {result.get('error', 'Unknown error')}")
```

### 3.2 Agent Integration Usage

```python
# Define a custom agent
class CustomBrowserAgent:
    def __init__(self, config=None):
        self.config = config or {}
        self.browser = self.config.get("browser", "chrome")
        
    def execute(self, request):
        # Extract action and parameters
        action = request.get("action", "open")
        url = request.get("url", "https://www.google.com")
        
        # Execute the action
        if action == "open":
            return self._open_browser(url)
        elif action == "search":
            return self._search(url)
        elif action == "close":
            return self._close_browser()
        else:
            return {
                "success": False,
                "error": f"Unknown action: {action}"
            }
            
    def _open_browser(self, url):
        # Implementation for opening browser
        print(f"Opening {url} in {self.browser}")
        return {"success": True}
        
    def _search(self, query):
        # Implementation for searching
        print(f"Searching for {query} in {self.browser}")
        return {"success": True}
        
    def _close_browser(self):
        # Implementation for closing browser
        print(f"Closing {self.browser}")
        return {"success": True}

# Register the custom agent
agent_registry = assistant.get_orchestration_layer().agent_registry
agent_registry.register_agent("custom_browser", CustomBrowserAgent, {"browser": "firefox"})

# Create and use the agent
browser_agent = agent_registry.create_agent("custom_browser")
result = browser_agent.execute({
    "action": "open",
    "url": "https://www.github.com"
})
```

### 3.3 Event-Based Integration Usage

```python
# Define event handlers
def on_user_request(data):
    print(f"Received user request: {data['content']}")
    
def on_tool_execution(data):
    print(f"Tool executed: {data['tool']}, Result: {data['success']}")
    
def on_agent_action(data):
    print(f"Agent action: {data['agent']}, Action: {data['action']}")
    
def on_guideline_violation(data):
    print(f"Guideline violation: {data['guideline']}, Rule: {data['rule']}")
    
def on_knowledge_query(data):
    print(f"Knowledge query: {data['query']}, Results: {len(data['results'])}")

# Register event handlers
event_manager = assistant.get_event_manager()
event_manager.register_handler("user_request", on_user_request)
event_manager.register_handler("tool_execution", on_tool_execution)
event_manager.register_handler("agent_action", on_agent_action)
event_manager.register_handler("guideline_violation", on_guideline_violation)
event_manager.register_handler("knowledge_query", on_knowledge_query)

# Emit events
event_manager.emit_event("user_request", {
    "content": "open Chrome and search for weather",
    "timestamp": "2023-08-27T14:30:00Z"
})

event_manager.emit_event("tool_execution", {
    "tool": "ui_click",
    "parameters": {"x": 100, "y": 200},
    "success": True
})
```

## 4. Advanced Usage Patterns

### 4.1 Multi-Agent Collaboration

```python
# Define a collaborative task
collaborative_task = {
    "task": "research_and_summarize",
    "query": "latest Windows 11 features",
    "output_format": "markdown"
}

# Create a multi-agent workflow
workflow = {
    "name": "research_workflow",
    "agents": [
        {
            "name": "researcher",
            "type": "browser_agent",
            "config": {
                "browser": "chrome",
                "search_engine": "google"
            }
        },
        {
            "name": "analyzer",
            "type": "rag_agent",
            "config": {
                "model": "gpt-4",
                "temperature": 0.3
            }
        },
        {
            "name": "writer",
            "type": "content_agent",
            "config": {
                "format": "markdown",
                "style": "technical"
            }
        }
    ],
    "flow": [
        {
            "agent": "researcher",
            "action": "search",
            "parameters": {
                "query": "{task.query}",
                "num_results": 5
            },
            "output": "search_results"
        },
        {
            "agent": "analyzer",
            "action": "analyze",
            "parameters": {
                "content": "{search_results}",
                "focus": "key features"
            },
            "output": "analysis"
        },
        {
            "agent": "writer",
            "action": "write",
            "parameters": {
                "content": "{analysis}",
                "format": "{task.output_format}",
                "title": "{task.query}"
            },
            "output": "final_document"
        }
    ]
}

# Execute the workflow
workflow_manager = assistant.get_orchestration_layer().workflow_manager
result = workflow_manager.execute_workflow(workflow, collaborative_task)

# Get the final output
final_document = result["final_document"]
print(final_document)
```

### 4.2 Guideline-Based Control

```python
# Define complex guidelines
guidelines = [
    {
        "id": "security_guidelines",
        "description": "Security guidelines for system operations",
        "rules": [
            {
                "id": "file_access",
                "description": "File access restrictions",
                "allowed_directories": [
                    "C:/Users/Public",
                    "C:/Program Files",
                    "{user_documents}"
                ],
                "forbidden_directories": [
                    "C:/Windows/System32",
                    "C:/Program Files/WindowsApps"
                ]
            },
            {
                "id": "network_access",
                "description": "Network access restrictions",
                "allowed_domains": [
                    "*.microsoft.com",
                    "*.github.com",
                    "*.google.com"
                ],
                "forbidden_domains": [
                    "*.suspicious-site.com"
                ]
            }
        ]
    },
    {
        "id": "privacy_guidelines",
        "description": "Privacy guidelines for user data",
        "rules": [
            {
                "id": "data_collection",
                "description": "Data collection restrictions",
                "allowed_data": [
                    "system_info",
                    "application_usage"
                ],
                "forbidden_data": [
                    "personal_files",
                    "credentials",
                    "browsing_history"
                ]
            }
        ]
    }
]

# Register guidelines
compliance_engine = assistant.get_compliance_layer().engine
for guideline in guidelines:
    compliance_engine.register_guideline(guideline)

# Create a file operation request
file_operation = {
    "type": "file_operation",
    "action": "read",
    "path": "C:/Windows/System32/config/system"
}

# Validate against guidelines
validation = compliance_engine.validate_action(file_operation)

# Check validation results
if not validation["compliant"]:
    print("File operation violates security guidelines:")
    for violation in validation["violations"]:
        print(f"- {violation['rule']}: {violation['description']}")
        
    # Suggest alternative
    suggestion = compliance_engine.suggest_alternative(file_operation)
    if suggestion:
        print(f"Suggested alternative: {suggestion}")
```

### 4.3 Knowledge Integration

```python
# Import document processing components
from archon.server.services.crawling import WebCrawler
from archon.server.services.knowledge import DocumentProcessor

# Initialize components
crawler = WebCrawler()
processor = DocumentProcessor()

# Crawl a website for knowledge
crawl_result = crawler.crawl({
    "url": "https://learn.microsoft.com/en-us/windows/",
    "depth": 2,
    "max_pages": 50,
    "filters": {
        "include_patterns": ["*/windows-11/*"],
        "exclude_patterns": ["*/previous-versions/*"]
    }
})

# Process crawled pages
for page in crawl_result["pages"]:
    document = {
        "title": page["title"],
        "content": page["content"],
        "url": page["url"],
        "type": "webpage"
    }
    
    # Process and index the document
    processor.process(document)

# Query the knowledge base
rag_agent = assistant.get_knowledge_layer().rag_engine
query_result = rag_agent.query("How to customize the Start menu in Windows 11?")

# Display results
print(f"Query: {query_result['query']}")
print(f"Answer: {query_result['answer']}")
print("Sources:")
for source in query_result["sources"]:
    print(f"- {source['title']} ({source['url']})")
```

## 5. UI Automation Usage

### 5.1 Screen Interaction

```python
# Import UI automation components
from pc_assistant.tools.ui_automation import ScreenInteraction

# Initialize screen interaction
screen = ScreenInteraction()

# Capture screen
screenshot = screen.capture()

# Find UI element
element = screen.find_element({
    "type": "button",
    "text": "Start",
    "region": [0, 0, 300, 300]
})

# Click on element
if element:
    screen.click(element["center_x"], element["center_y"])
    
# Type text
screen.type_text("notepad")

# Press key combination
screen.press_keys(["enter"])

# Wait for element to appear
notepad_window = screen.wait_for_element({
    "type": "window",
    "title": "Notepad"
}, timeout=5)

# Type in notepad
if notepad_window:
    screen.type_text("Hello, this is a test.")
    
# Save file
screen.press_keys(["ctrl", "s"])
save_dialog = screen.wait_for_element({
    "type": "dialog",
    "title": "Save As"
}, timeout=5)

if save_dialog:
    screen.type_text("test_file.txt")
    screen.press_keys(["enter"])
```

### 5.2 Application Control

```python
# Import application control components
from pc_assistant.tools.application_control import ApplicationManager

# Initialize application manager
app_manager = ApplicationManager()

# Launch application
chrome = app_manager.launch("chrome")

# Navigate to URL
if chrome:
    chrome.navigate("https://www.github.com")
    
# Wait for page to load
chrome.wait_for_page_load()

# Find and click element
login_button = chrome.find_element({
    "type": "button",
    "text": "Sign in"
})

if login_button:
    chrome.click(login_button)
    
# Fill form
chrome.fill_form({
    "username": "user@example.com",
    "password": "password123"
})

# Submit form
chrome.submit_form()

# Close application
chrome.close()
```

### 5.3 System Integration

```python
# Import system integration components
from pc_assistant.tools.system_integration import SystemManager

# Initialize system manager
system = SystemManager()

# Get system information
system_info = system.get_info()
print(f"OS: {system_info['os']}")
print(f"Version: {system_info['version']}")
print(f"Memory: {system_info['memory']} GB")
print(f"CPU: {system_info['cpu']}")

# Execute system command
result = system.execute_command("dir C:\\Users\\Public")
print(result["output"])

# Manage files
files = system.list_files("C:\\Users\\Public")
for file in files:
    print(f"{file['name']} - {file['size']} bytes")
    
# Create directory
system.create_directory("C:\\Users\\Public\\TestDir")

# Copy file
system.copy_file("C:\\Users\\Public\\test_file.txt", "C:\\Users\\Public\\TestDir\\test_file_copy.txt")

# Get process list
processes = system.get_processes()
for process in processes[:5]:  # Show first 5 processes
    print(f"{process['name']} (PID: {process['pid']}) - Memory: {process['memory']} MB")
```

## 6. WSL2 Integration Usage

### 6.1 Command Execution

```python
# Import WSL2 integration components
from pc_assistant.tools.wsl2_integration import WSL2Manager

# Initialize WSL2 manager
wsl = WSL2Manager()

# Execute command in WSL2
result = wsl.execute_command("ls -la")
print(result["output"])

# Run Python script in WSL2
script_result = wsl.run_python_script("my_script.py", {
    "param1": "value1",
    "param2": "value2"
})

# Install package in WSL2
wsl.install_package("numpy")

# Get WSL2 system information
wsl_info = wsl.get_system_info()
print(f"Distribution: {wsl_info['distribution']}")
print(f"Kernel: {wsl_info['kernel']}")
```

### 6.2 File Transfer

```python
# Transfer file from Windows to WSL2
wsl.copy_to_wsl("C:\\Users\\Public\\test_file.txt", "/home/user/test_file.txt")

# Transfer file from WSL2 to Windows
wsl.copy_from_wsl("/home/user/output.txt", "C:\\Users\\Public\\output.txt")

# Mount Windows drive in WSL2
wsl.mount_windows_drive("C", "/mnt/c")

# Access Windows file from WSL2
result = wsl.execute_command("cat /mnt/c/Users/Public/test_file.txt")
print(result["output"])
```

## 7. Conclusion

The usage schemes presented in this document provide comprehensive patterns for utilizing the integrated Windows PC assistant that combines Archon, OxyGent, and parlant frameworks. These schemes cover:

1. **Core Usage Patterns**: Basic interaction flows, command line usage, and API usage
2. **Framework-Specific Usage**: Utilizing the unique capabilities of each framework
3. **Integration Usage Patterns**: Tool integration, agent integration, and event-based integration
4. **Advanced Usage Patterns**: Multi-agent collaboration, guideline-based control, and knowledge integration
5. **UI Automation Usage**: Screen interaction, application control, and system integration
6. **WSL2 Integration Usage**: Command execution and file transfer

By following these usage schemes, developers and users can effectively leverage the full capabilities of the integrated Windows PC assistant for a wide range of tasks and scenarios.

