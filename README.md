# mcp_project
MCP project repository hosts the source code for the demostration of MCP servers and clients.

## Workflow Overview

### System Architecture
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  Claude Desktop │    │  MCP Inspector  │    │   Research      │
│                 │◄──►│                 │◄──►│   Server        │
│  - Chat Client  │    │  - Debug Tool   │    │  - arXiv API    │
│  - MCP Client   │    │  - Test Server  │    │  - File Storage │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                        │                        │
         │                        │                        │
         ▼                        ▼                        ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ Configuration   │    │    Network      │    │   Local Files   │
│                 │    │                 │    │                 │
│claude_desktop_  │    │   HTTP/STDIO    │    │   papers/       │
│config.json      │    │   Transport     │    │   ├─topic/      │
└─────────────────┘    └─────────────────┘    │   └─papers.json │
                                              └─────────────────┘
```

### Paper Search Workflow
```
User Query ──► Claude Desktop ──► MCP Server ──► arXiv API
    │              │                  │             │
    │              │                  │             ▼
    │              │                  │        Search Results
    │              │                  │             │
    │              │                  │             ▼
    │              │                  ▼        Parse Papers
    │              │            Store Locally       │
    │              │                  │             │
    │              │                  ▼             ▼
    │              ▼            papers/topic/   JSON Format
    ▼         Response            papers.json       │
Results   ◄─────────────────────────────────────────┘
```

### File Organization Flow
```
search_papers("machine learning", 5)
                    │
                    ▼
            ┌───────────────┐
            │  Create Dir   │
            │ papers/       │
            │ machine_      │
            │ learning/     │
            └───────┬───────┘
                    │
                    ▼
            ┌───────────────┐
            │ Download &    │
            │ Parse Papers  │
            │ from arXiv    │
            └───────┬───────┘
                    │
                    ▼
            ┌───────────────┐
            │ Save to       │
            │ papers_info.  │
            │ json          │
            └───────────────┘
```

### MCP Tool Interaction Flow
```
┌─────────────┐    Tool: search_papers()
│   Claude    │───────────────────────┐
│             │                       │
│   Request   │   Tool: extract_info()│
│             │◄──────────────────────┤
└─────────────┘                       │
       │                              │
       ▼                              │
┌─────────────┐ Resource: papers://folders  
│    MCP      │◄──────────────────────┘
│   Server    │                       │
│             │    Resource: papers://{topic}
│             │◄──────────────────────┘
└─────────────┘
       │
       ▼
┌─────────────┐
│   File      │
│   System    │
│             │
└─────────────┘
```

## Steps to quickly setup the MCP demo

### Step 1: Development Environment Setup
This project requires Python 3.8 or higher. You can download Python [here](#https://www.python.org/downloads) for Windows. First, you need to install uv and nodejs. Second, create *.venv* python environment.
 ```bat
cd mcp_project
uv init
uv venv
.venv\Scripts\activate
uv add mcp arxiv anthropic python-dotenv nest_asyncio
```

Third, MCP Inspector is an interactive developer tool for testing and debugging MCP servers. Run the Inspector thorough the following command.
 ```bat
npx @modelcontextprotocol/inspector uv run research_server.py
```

### Step 2: Cloude Desktop Setup
For installling Claude Desktop, read [Coude Support](#https://support.claude.com/en/articles/10065433-installing-claude-desktop).
Then, copy the claude_desktop_config.json to Windows:
 ```bat
%APPDATA%\Claude\claude_desktop_config.json
```

## Development Workflows

### Development Setup Flow
```
Start Here
    │
    ▼
┌─────────────────┐
│ Install Python  │
│    (3.8+)       │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│ Install uv &    │
│    Node.js      │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│ Create Virtual  │
│  Environment    │
│   (.venv)       │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│ Install MCP     │
│ Dependencies    │
│(mcp, arxiv...)  │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│ Test Server     │
│ with Inspector  │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│ Configure       │
│ Claude Desktop  │
└─────────────────┘
```

### Testing Workflow
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│     Method 1    │    │     Method 2    │    │     Method 3    │
│                 │    │                 │    │                 │
│ MCP Inspector   │    │ Claude Desktop  │    │ Direct Python   │
│                 │    │                 │    │                 │
│ ┌─────────────┐ │    │ ┌─────────────┐ │    │ ┌─────────────┐ │
│ │Start Server │ │    │ │Config File  │ │    │ │Command Line │ │
│ │with Inspector │    │ │Integration  │ │    │ │Testing      │ │
│ └─────────────┘ │    │ └─────────────┘ │    │ └─────────────┘ │
│                 │    │                 │    │                 │
│ ┌─────────────┐ │    │ ┌─────────────┐ │    │ ┌─────────────┐ │
│ │Interactive  │ │    │ │Real Usage   │ │    │ │Debug Mode   │ │
│ │Web UI       │ │    │ │Environment  │ │    │ │Direct Call  │ │
│ └─────────────┘ │    │ └─────────────┘ │    │ └─────────────┘ │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### Data Flow Diagram
```
User Query: "Find papers on quantum computing"
            │
            ▼
    ┌──────────────┐
    │ Claude       │
    │ Desktop      │
    │              │
    └──────┬───────┘
           │ MCP Protocol
           │ (JSON-RPC)
           ▼
    ┌──────────────┐
    │ Research     │
    │ Server       │
    │ (FastMCP)    │
    └──────┬───────┘
           │
           ▼
    ┌──────────────┐         ┌─────────────┐
    │ search_      │────────►│ arXiv API   │
    │ papers()     │         │             │
    │              │◄────────│ GET papers  │
    └──────┬───────┘         └─────────────┘
           │
           ▼
    ┌──────────────┐         ┌─────────────┐
    │ Process &    │────────►│ File System │
    │ Format       │         │             │
    │ Results      │         │papers/topic/│
    └──────┬───────┘         │papers.json  │
           │                 └─────────────┘
           ▼
    ┌──────────────┐
    │ Return to    │
    │ Claude       │
    │              │
    └──────────────┘
```





 


 





 

 
