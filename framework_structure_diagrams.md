# Framework Structure Diagrams

This document provides detailed structure diagrams for Archon, OxyGent, and parlant frameworks, highlighting their core components, class hierarchies, and relationships.

## 1. Archon Structure

### 1.1 Component Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                         Archon                              │
└─────────────────────────────────────────────────────────────┘
                              │
                 ┌────────────┴────────────┐
                 ▼                         ▼
┌────────────────────────┐    ┌────────────────────────┐
│      Frontend          │    │      Backend           │
│  (archon-ui-main)      │    │      (python)          │
└────────────┬───────────┘    └────────────┬───────────┘
             │                             │
             ▼                             ▼
┌────────────────────────┐    ┌────────────────────────┐
│  React Components      │    │  Server                │
├────────────────────────┤    ├────────────────────────┤
│  - Knowledge Base      │    │  - FastAPI Routes      │
│  - MCP Integration     │    │  - Socket.IO           │
│  - Project Management  │    │  - Services            │
│  - Settings            │    │  - Middleware          │
└────────────────────────┘    └────────────┬───────────┘
                                           │
                              ┌────────────┴────────────┐
                              ▼                         ▼
                 ┌────────────────────┐    ┌────────────────────┐
                 │   MCP Server       │    │   Agents Service   │
                 ├────────────────────┤    ├────────────────────┤
                 │  - Tool Registry   │    │  - Base Agent      │
                 │  - Features        │    │  - Document Agent  │
                 │  - Modules         │    │  - RAG Agent       │
                 └────────────────────┘    └────────────────────┘
```

### 1.2 Class Hierarchy

```
BaseAgent
  ├── DocumentAgent
  │     └── Methods:
  │         ├── process_document()
  │         ├── extract_content()
  │         └── index_document()
  │
  ├── RAGAgent
  │     └── Methods:
  │         ├── query()
  │         ├── retrieve_context()
  │         └── generate_response()
  │
  └── MCPClient
        └── Methods:
            ├── connect()
            ├── execute_tool()
            └── get_available_tools()

MCP Server
  ├── Features
  │     ├── Documents
  │     │     └── Tools:
  │     │         ├── document_tools
  │     │         └── version_tools
  │     │
  │     ├── Projects
  │     │     └── Tools:
  │     │         └── project_tools
  │     │
  │     └── Tasks
  │           └── Tools:
  │               └── task_tools
  │
  └── Modules
        ├── Models
        └── RAG Module
```

### 1.3 Data Flow

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│  User    │     │ Frontend │     │  Server  │     │  MCP     │
│ Interface│     │  (React) │     │ (FastAPI)│     │  Server  │
└────┬─────┘     └────┬─────┘     └────┬─────┘     └────┬─────┘
     │                │                │                │
     │ User Input     │                │                │
     │───────────────>│                │                │
     │                │                │                │
     │                │ API Request    │                │
     │                │───────────────>│                │
     │                │                │                │
     │                │                │ MCP Tool Call  │
     │                │                │───────────────>│
     │                │                │                │
     │                │                │                │
     │                │                │ Tool Result    │
     │                │                │<───────────────│
     │                │                │                │
     │                │ API Response   │                │
     │                │<───────────────│                │
     │                │                │                │
     │ UI Update      │                │                │
     │<───────────────│                │                │
     │                │                │                │
```

## 2. OxyGent Structure

### 2.1 Component Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                         OxyGent                             │
└─────────────────────────────────────────────────────────────┘
                              │
                 ┌────────────┴────────────┐
                 ▼                         ▼
┌────────────────────────┐    ┌────────────────────────┐
│      Core (oxy)        │    │      Databases         │
└────────────┬───────────┘    └────────────┬───────────┘
             │                             │
    ┌────────┴────────┐           ┌───────┴────────┐
    ▼                 ▼           ▼                ▼
┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
│ Agents  │    │ Flows   │    │Vector DB│    │Redis/ES │
└────┬────┘    └────┬────┘    └─────────┘    └─────────┘
     │              │
     ▼              ▼
┌─────────────┐ ┌─────────────┐
│BaseAgent    │ │BaseFlow     │
├─────────────┤ ├─────────────┤
│ChatAgent    │ │ParallelFlow │
│ReactAgent   │ │PlanAndSolve │
│ParallelAgent│ │Reflexion    │
│WorkflowAgent│ │Workflow     │
└─────────────┘ └─────────────┘
```

### 2.2 Class Hierarchy

```
BaseAgent
  ├── ChatAgent
  │     └── Methods:
  │         ├── chat()
  │         ├── generate_response()
  │         └── process_history()
  │
  ├── ReactAgent
  │     └── Methods:
  │         ├── think()
  │         ├── act()
  │         └── observe()
  │
  ├── ParallelAgent
  │     └── Methods:
  │         ├── distribute_tasks()
  │         ├── collect_results()
  │         └── aggregate()
  │
  ├── WorkflowAgent
  │     └── Methods:
  │         ├── execute_workflow()
  │         ├── track_progress()
  │         └── handle_transitions()
  │
  ├── LocalAgent
  │     └── Methods:
  │         ├── execute_locally()
  │         └── process_result()
  │
  └── RemoteAgent
        └── Methods:
            ├── connect_remote()
            ├── send_request()
            └── receive_response()

BaseFlow
  ├── ParallelFlow
  │     └── Methods:
  │         ├── distribute()
  │         └── aggregate()
  │
  ├── PlanAndSolve
  │     └── Methods:
  │         ├── create_plan()
  │         └── execute_steps()
  │
  ├── Reflexion
  │     └── Methods:
  │         ├── reflect()
  │         └── improve()
  │
  └── Workflow
        └── Methods:
            ├── define_steps()
            ├── execute_step()
            └── transition()

BaseTool
  ├── FunctionTool
  │     └── Methods:
  │         ├── register_function()
  │         └── execute()
  │
  ├── APITool
  │     └── Methods:
  │         ├── make_request()
  │         └── parse_response()
  │
  └── MCPTool
        └── Methods:
            ├── connect_mcp()
            ├── execute_tool()
            └── handle_result()
```

### 2.3 Data Flow

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│  User    │     │ OxyGent  │     │  Flow    │     │  Agent   │
│ Request  │     │ Core     │     │ Manager  │     │ Executor │
└────┬─────┘     └────┬─────┘     └────┬─────┘     └────┬─────┘
     │                │                │                │
     │ Input          │                │                │
     │───────────────>│                │                │
     │                │                │                │
     │                │ Select Flow    │                │
     │                │───────────────>│                │
     │                │                │                │
     │                │                │ Assign Agents  │
     │                │                │───────────────>│
     │                │                │                │
     │                │                │                │
     │                │                │ Execute Tools  │
     │                │                │<───────────────│
     │                │                │                │
     │                │ Flow Result    │                │
     │                │<───────────────│                │
     │                │                │                │
     │ Response       │                │                │
     │<───────────────│                │                │
     │                │                │                │
```

## 3. parlant Structure

### 3.1 Component Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                         parlant                             │
└─────────────────────────────────────────────────────────────┘
                              │
         ┌──────────┬─────────┴─────────┬──────────┐
         ▼          ▼                   ▼          ▼
┌────────────┐ ┌────────────┐    ┌────────────┐ ┌────────────┐
│   Core     │ │  Adapters  │    │    API     │ │   Bin      │
└─────┬──────┘ └─────┬──────┘    └─────┬──────┘ └────────────┘
      │              │                 │
      ▼              ▼                 ▼
┌────────────┐ ┌────────────┐    ┌────────────┐
│  Engines   │ │    DB      │    │   Chat     │
├────────────┤ ├────────────┤    └────────────┘
│  Services  │ │  Loggers   │
├────────────┤ ├────────────┤
│ Persistence│ │    NLP     │
└────────────┘ └────────────┘
```

### 3.2 Class Hierarchy

```
Engine (Alpha)
  ├── GuidelineMatcher
  │     └── Methods:
  │         ├── match()
  │         ├── rank_matches()
  │         └── apply_guidelines()
  │
  ├── ToolCaller
  │     └── Methods:
  │         ├── call_tool()
  │         ├── validate_tool_call()
  │         └── process_result()
  │
  ├── MessageGenerator
  │     └── Methods:
  │         ├── generate()
  │         ├── apply_guidelines()
  │         └── format_response()
  │
  └── PromptBuilder
        └── Methods:
            ├── build()
            ├── add_context()
            └── add_guidelines()

GuidelineMatching
  ├── GenericGuidelineMatchingStrategy
  │     └── Methods:
  │         ├── match()
  │         └── score()
  │
  ├── DisambiguationBatch
  │     └── Methods:
  │         ├── process()
  │         └── disambiguate()
  │
  ├── GuidelineActionableBatch
  │     └── Methods:
  │         ├── process()
  │         └── determine_actions()
  │
  └── ObservationalBatch
        └── Methods:
            ├── process()
            └── observe()

ToolCalling
  ├── DefaultToolCallBatcher
  │     └── Methods:
  │         ├── batch()
  │         └── prioritize()
  │
  ├── OverlappingToolsBatch
  │     └── Methods:
  │         ├── process()
  │         └── resolve_overlaps()
  │
  └── SingleToolBatch
        └── Methods:
            ├── process()
            └── execute()
```

### 3.3 Data Flow

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│  User    │     │  Engine  │     │ Guideline│     │  Tool    │
│ Message  │     │  (Alpha) │     │ Matcher  │     │  Caller  │
└────┬─────┘     └────┬─────┘     └────┬─────┘     └────┬─────┘
     │                │                │                │
     │ Input          │                │                │
     │───────────────>│                │                │
     │                │                │                │
     │                │ Match Guidelines                │
     │                │───────────────>│                │
     │                │                │                │
     │                │ Guideline Matches               │
     │                │<───────────────│                │
     │                │                │                │
     │                │ Call Tools     │                │
     │                │───────────────────────────────>│
     │                │                │                │
     │                │                │ Tool Results   │
     │                │<───────────────────────────────│
     │                │                │                │
     │ Response       │                │                │
     │<───────────────│                │                │
     │                │                │                │
```

## 4. Integrated System Structure

### 4.1 Component Integration

```
┌─────────────────────────────────────────────────────────────┐
│                    Windows PC Assistant                     │
└─────────────────────────────────────────────────────────────┘
                              │
         ┌──────────┬─────────┴─────────┬──────────┐
         ▼          ▼                   ▼          ▼
┌────────────┐ ┌────────────┐    ┌────────────┐ ┌────────────┐
│ OxyGent    │ │  parlant   │    │  Archon    │ │ MCP Tools  │
│ Orchestrator│ │ Compliance │    │ Knowledge  │ │ Execution  │
└─────┬──────┘ └─────┬──────┘    └─────┬──────┘ └─────┬──────┘
      │              │                 │              │
      ▼              ▼                 ▼              ▼
┌────────────┐ ┌────────────┐    ┌────────────┐ ┌────────────┐
│ Agent      │ │ Guideline  │    │ Knowledge  │ │ UI         │
│ Management │ │ Enforcement│    │ Retrieval  │ │ Automation │
├────────────┤ ├────────────┤    ├────────────┤ ├────────────┤
│ Flow       │ │ Behavior   │    │ Document   │ │ System     │
│ Execution  │ │ Control    │    │ Processing │ │ Commands   │
├────────────┤ ├────────────┤    ├────────────┤ ├────────────┤
│ Event      │ │ Validation │    │ MCP Server │ │ Application│
│ Management │ │ Mechanisms │    │ Integration│ │ Integration│
└────────────┘ └────────────┘    └────────────┘ └────────────┘
```

### 4.2 Class Integration

```
PCAssistantOrchestrator
  ├── OxyFlowManager
  │     └── Methods:
  │         ├── select_flow()
  │         ├── execute_flow()
  │         └── monitor_progress()
  │
  ├── parlantComplianceEngine
  │     └── Methods:
  │         ├── validate_request()
  │         ├── validate_action()
  │         └── enforce_guidelines()
  │
  ├── ArchonKnowledgeManager
  │     └── Methods:
  │         ├── get_context()
  │         ├── index_content()
  │         └── retrieve_knowledge()
  │
  └── MCPToolRegistry
        └── Methods:
            ├── register_tool()
            ├── get_tool()
            └── execute_tool()
```

### 4.3 Integrated Data Flow

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│  User    │     │ OxyGent  │     │  parlant │     │  Archon  │
│ Request  │     │ Orchestr.│     │ Compliance│    │ Knowledge│
└────┬─────┘     └────┬─────┘     └────┬─────┘     └────┬─────┘
     │                │                │                │
     │ Input          │                │                │
     │───────────────>│                │                │
     │                │                │                │
     │                │ Validate Request               │
     │                │───────────────>│                │
     │                │                │                │
     │                │ Validation Result              │
     │                │<───────────────│                │
     │                │                │                │
     │                │ Get Knowledge Context          │
     │                │───────────────────────────────>│
     │                │                │                │
     │                │                │ Knowledge Data │
     │                │<───────────────────────────────│
     │                │                │                │
     │                │ Execute Flow   │                │
     │                │───────────────>│                │
     │                │                │                │
     │                │ Validate Actions               │
     │                │<───────────────│                │
     │                │                │                │
     │                │ Execute MCP Tools              │
     │                │───────────────────────────────>│
     │                │                │                │
     │                │                │ Tool Results   │
     │                │<───────────────────────────────│
     │                │                │                │
     │ Response       │                │                │
     │<───────────────│                │                │
     │                │                │                │
```

## 5. Windows PC Assistant Implementation

### 5.1 System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      Windows PC Host                        │
└─────────────────────────────────────────────────────────────┘
                              │
         ┌──────────┬─────────┴─────────┬──────────┐
         ▼          ▼                   ▼          ▼
┌────────────┐ ┌────────────┐    ┌────────────┐ ┌────────────┐
│ Assistant  │ │ UI         │    │ System     │ │ WSL2       │
│ Core       │ │ Automation │    │ Integration│ │ Bridge     │
└─────┬──────┘ └─────┬──────┘    └─────┬──────┘ └─────┬──────┘
      │              │                 │              │
      ▼              ▼                 ▼              ▼
┌────────────┐ ┌────────────┐    ┌────────────┐ ┌────────────┐
│ OxyGent    │ │ Screen     │    │ File       │ │ Command    │
│ Integration│ │ Capture    │    │ Operations │ │ Execution  │
├────────────┤ ├────────────┤    ├────────────┤ ├────────────┤
│ parlant    │ │ Mouse/     │    │ Process    │ │ Knowledge  │
│ Integration│ │ Keyboard   │    │ Management │ │ Services   │
├────────────┤ ├────────────┤    ├────────────┤ ├────────────┤
│ Archon     │ │ UI Element │    │ Registry   │ │ Compliance │
│ Integration│ │ Detection  │    │ Access     │ │ Engine     │
└────────────┘ └────────────┘    └────────────┘ └────────────┘
```

### 5.2 Implementation Classes

```
WindowsPCAssistant
  ├── Core
  │     ├── AssistantOrchestrator
  │     ├── EventManager
  │     ├── ConfigurationManager
  │     └── SessionManager
  │
  ├── UI Automation
  │     ├── ScreenCapture
  │     ├── MouseController
  │     ├── KeyboardController
  │     └── UIElementDetector
  │
  ├── System Integration
  │     ├── FileOperations
  │     ├── ProcessManager
  │     ├── RegistryAccess
  │     └── NetworkOperations
  │
  ├── WSL2 Bridge
  │     ├── CommandExecutor
  │     ├── FileTransfer
  │     ├── ServiceCommunication
  │     └── EnvironmentManager
  │
  └── MCP Integration
        ├── ToolRegistry
        ├── ToolExecutor
        ├── ResultProcessor
        └── ToolValidator
```

### 5.3 Deployment Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      Windows PC Host                        │
│                                                             │
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────────┐   │
│  │ Assistant   │   │ MCP Server  │   │ UI Automation   │   │
│  │ Core        │◄──┼──►(Windows) │   │ Service         │   │
│  └──────┬──────┘   └─────────────┘   └────────┬────────┘   │
│         │                                      │            │
│         └──────────────────┬───────────────────┘            │
│                            │                                │
│  ┌─────────────────────────────────────────────────────┐   │
│  │                    WSL2 Instance                     │   │
│  │                                                     │   │
│  │  ┌─────────────┐   ┌─────────────┐   ┌───────────┐ │   │
│  │  │ Knowledge   │   │ Compliance  │   │ Command   │ │   │
│  │  │ Services    │◄──┼──►Engine    │◄──┼─►Execution│ │   │
│  │  └─────────────┘   └─────────────┘   └───────────┘ │   │
│  │                                                     │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 6. Integration Points

### 6.1 Framework Communication

```
┌────────────┐                              ┌────────────┐
│  OxyGent   │                              │  parlant   │
│ Orchestrator│                              │ Compliance │
└─────┬──────┘                              └─────┬──────┘
      │                                           │
      │ ◄─── Agent Selection & Orchestration ───►│
      │                                           │
      │ ◄─── Guideline Validation ──────────────►│
      │                                           │
      │ ◄─── Action Approval ──────────────────►│
      │                                           │
      │ ◄─── Event Notification ───────────────►│
      │                                           │
      ▼                                           ▼
┌────────────┐                              ┌────────────┐
│  Archon    │                              │  MCP Tool  │
│ Knowledge  │                              │ Execution  │
└─────┬──────┘                              └─────┬──────┘
      │                                           │
      │ ◄─── Knowledge Retrieval ───────────────►│
      │                                           │
      │ ◄─── Document Processing ───────────────►│
      │                                           │
      │ ◄─── MCP Tool Registration ─────────────►│
      │                                           │
      │ ◄─── Tool Execution ─────────────────────►│
      │                                           │
```

### 6.2 Data Exchange Format

```json
{
  "request": {
    "id": "req-123456",
    "type": "user_command",
    "content": "Open Chrome and search for weather",
    "timestamp": "2023-08-27T14:30:00Z",
    "metadata": {
      "source": "chat_interface",
      "priority": "normal"
    }
  },
  
  "context": {
    "session_id": "sess-789012",
    "user_id": "user-345678",
    "history": [
      {
        "role": "user",
        "content": "I need to check the weather",
        "timestamp": "2023-08-27T14:29:45Z"
      }
    ],
    "system_state": {
      "running_applications": ["notepad.exe", "explorer.exe"],
      "active_window": "chat_interface",
      "screen_resolution": "1920x1080"
    }
  },
  
  "orchestration": {
    "flow_id": "flow-234567",
    "flow_type": "ui_automation",
    "agents": [
      {
        "id": "agent-456789",
        "type": "ui_automation_agent",
        "status": "assigned"
      },
      {
        "id": "agent-567890",
        "type": "browser_control_agent",
        "status": "standby"
      }
    ],
    "steps": [
      {
        "id": "step-678901",
        "description": "Launch Chrome browser",
        "status": "pending"
      },
      {
        "id": "step-789012",
        "description": "Navigate to search engine",
        "status": "pending"
      },
      {
        "id": "step-890123",
        "description": "Search for weather information",
        "status": "pending"
      }
    ]
  },
  
  "compliance": {
    "guidelines_applied": [
      {
        "id": "guideline-901234",
        "name": "browser_usage_policy",
        "status": "compliant"
      },
      {
        "id": "guideline-012345",
        "name": "search_privacy_policy",
        "status": "compliant"
      }
    ],
    "validation_result": "approved",
    "restrictions": []
  },
  
  "knowledge": {
    "relevant_documents": [
      {
        "id": "doc-123456",
        "title": "Chrome Browser Usage Guide",
        "relevance_score": 0.85
      }
    ],
    "context_items": [
      {
        "type": "browser_command",
        "content": "Chrome can be launched via Start Menu or taskbar"
      }
    ]
  },
  
  "tools": {
    "executed": [
      {
        "id": "tool-234567",
        "name": "launch_application",
        "parameters": {
          "application_name": "chrome.exe"
        },
        "status": "success",
        "result": {
          "process_id": 12345,
          "window_handle": "0x1A2B3C"
        }
      }
    ],
    "pending": [
      {
        "id": "tool-345678",
        "name": "browser_navigate",
        "parameters": {
          "url": "https://www.google.com"
        }
      }
    ]
  },
  
  "response": {
    "status": "in_progress",
    "message": "I'm opening Chrome and will search for the weather for you.",
    "completion_percentage": 33,
    "next_action": "navigate_to_search"
  }
}
```

## 7. Conclusion

The structure diagrams presented in this document provide a comprehensive view of the three frameworks (Archon, OxyGent, and parlant) and their integration into a Windows PC assistant. The diagrams highlight:

1. **Component Architecture**: The key components and their relationships within each framework
2. **Class Hierarchies**: The inheritance and composition patterns in each framework
3. **Data Flow**: How information moves through the components
4. **Integration Points**: How the frameworks connect and communicate
5. **Implementation Structure**: The practical organization of the integrated system

This architectural understanding forms the foundation for implementing a powerful Windows PC assistant that leverages the strengths of each framework while maintaining a cohesive and maintainable system structure.

