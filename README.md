# mcp_project
MCP project repository hosts the source code for the demostration of MCP servers and clients.

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





 


 





 

 
