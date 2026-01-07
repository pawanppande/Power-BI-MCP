# Power BI Integration with Claude via MCP (Model Context Protocol)

## Overview

Before starting this project, I would like to thank **Maxim Anatsko** ([maxanatsko.com](https://maxanatsko.com/)) for creating this excellent Power BI MCP extension. Full credit and documentation available at: [pbi-desktop-mcp-public](https://github.com/maxanatsko/pbi-desktop-mcp-public/tree/main)

This project demonstrates how to connect Claude AI with Power BI Desktop using the Model Context Protocol (MCP). This integration enables you to interact with Power BI models through natural language, allowing you to query data, create measures, manage relationships, and perform various Power BI operations conversationally.

## Table of Contents

- [Prerequisites](#prerequisites)
- [What is MCP?](#what-is-mcp)
- [Installation & Setup](#installation--setup)
- [Connecting Claude to Power BI](#connecting-claude-to-power-bi)
- [Features & Capabilities](#features--capabilities)
- [Usage Examples](#usage-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## Prerequisites

Before getting started, ensure you have the following:

- **Power BI Desktop** installed on your Windows machine
- **Claude Desktop app** - [Download here](https://claude.ai/download)
- **Node.js** (version 16 or later) - [Download here](https://nodejs.org/)
- Basic understanding of Power BI and DAX
- Administrator access to install components

## What is MCP?

**Model Context Protocol (MCP)** is an open protocol developed by Anthropic that enables AI assistants like Claude to securely connect to external data sources and tools. In this case, MCP acts as a bridge between Claude and Power BI Desktop, allowing Claude to:

- Read and analyze Power BI data models
- Execute DAX queries
- Create and modify measures, tables, and relationships
- Perform data transformations
- Manage model metadata

## Installation & Setup

### Step 1: Install Power BI Desktop

1. Download Power BI Desktop from [Microsoft's official website](https://powerbi.microsoft.com/desktop/)
2. Install and launch Power BI Desktop
3. Open or create a Power BI model (.pbix file)

### Step 2: Install MCP Power BI Extension

> **⚠️ Important:** This extension is not available in the public npm registry. Follow the direct installation method below.

#### Method 1: Direct Installation via .mcpb File (Recommended)

1. **Download the Extension**
   - Visit: [Releases/powerbi-desktop-mcp-2.0.10.mcpb](https://github.com/maxanatsko/pbi-desktop-mcp-public/blob/main/Releases/powerbi-desktop-mcp-2.0.10.mcpb)
   - Download the latest `.mcpb` file
   - Save it to a location you can easily access

2. **Install in Claude Desktop**
   - Open Claude Desktop app
   - Go to **Settings** → **Extensions** → **Advanced Settings**
   - Click **"Install Extension"**
   - Select the downloaded `.mcpb` file
   - Wait for installation to complete

3. **Restart Claude Desktop**
   - Completely quit Claude Desktop (check system tray)
   - Restart the application

#### Method 2: Manual Build from Source (Advanced Users)

If you prefer to build from source:

1. **Clone the Repository**
```bash
git clone https://github.com/maxanatsko/pbi-desktop-mcp-public.git
cd pbi-desktop-mcp-public
```

2. **Install Dependencies**
```bash
npm install
```

3. **Build the Extension**
```bash
npm run build
```

4. **Configure Manually** (see Step 3, Option B below)

### Step 3: Configure Claude Desktop (Optional for Method 1)

#### If You Used Method 1 (.mcpb installation):
✅ **No additional configuration needed!** Skip to Step 4.

#### If You Used Method 2 (Manual build):

**Option B: Manual Configuration File Setup**

1. **Locate Configuration File:**
   - **Windows**: Press `Win + R`, type `%APPDATA%\Claude`, press Enter
   - Look for `claude_desktop_config.json`
   - If it doesn't exist, create it

2. **Edit Configuration File:**
   - Open `claude_desktop_config.json` in a text editor
   - Add the following configuration:

```json
{
  "mcpServers": {
    "powerbi": {
      "command": "node",
      "args": [
        "C:\\path\\to\\pbi-desktop-mcp-public\\dist\\index.js"
      ]
    }
  }
}
```

3. **Update the Path:**
   - Replace `C:\\path\\to\\pbi-desktop-mcp-public\\dist\\index.js` with your actual build path
   - Use double backslashes `\\` in Windows paths
   - Example: `"C:\\Users\\YourName\\pbi-desktop-mcp-public\\dist\\index.js"`

4. **Save and Restart:**
   - Save the configuration file
   - Restart Claude Desktop completely

### Step 4: Verify Installation

1. **Open Claude Desktop**
2. **Start a New Conversation**
3. **Test MCP Connection:**
   ```
   Can you connect with MCP?
   ```
4. **Expected Response:**
   - Claude should confirm it has access to MCP
   - Should mention the Power BI Desktop MCP server
   - Should list available tools and capabilities

5. **Verify Extension:**
   - Go to Claude Desktop → Settings → Extensions
   - You should see "powerbi" or "Power BI Desktop" listed
   - Status should show as "Connected" or "Active"

## Connecting Claude to Power BI

### Opening Your Power BI Model

1. **Launch Power BI Desktop** and open your .pbix file
2. The Power BI model must be open and active for Claude to detect it
3. Keep Power BI Desktop running in the background

### Establishing Connection Through Claude

1. **In Claude, ask to connect:**
   ```
   Can you connect to my Power BI model?
   ```

2. **Claude will detect available models:**
   ```
   I can see you have a Power BI model called "YourModelName" available.
   ```

3. **Confirm the connection:**
   ```
   Yes, please connect to that model
   ```

4. **Claude will establish the connection and confirm success**

### Verifying the Connection

Once connected, you can verify by asking Claude to:
```
Show me all the tables in my Power BI model
```

```
List all measures
```

```
Show me the relationships
```


## Troubleshooting

### Issue: Extension Not Showing in Claude Desktop

**Symptoms:**
- No "powerbi" extension visible in Extensions list
- Claude doesn't recognize MCP connection

**Solutions:**
1. Verify the `.mcpb` file was installed correctly
   - Check: Settings → Extensions in Claude Desktop
   - Should see "powerbi" extension listed
2. Ensure you restarted Claude Desktop after installation
3. Try reinstalling the extension:
   - Remove existing extension if present
   - Reinstall from the `.mcpb` file
4. Check Claude Desktop version is up to date

### Issue: Claude Cannot Detect Power BI Model

**Symptoms:**
- "No Power BI instances found" message
- Claude connects to MCP but sees no models

**Solutions:**
1. **Ensure Power BI Desktop is running**
2. **Verify a `.pbix` file is actually open** (not just Power BI Desktop)
3. **Check the model is not in read-only mode**
4. **Try these steps:**
   - Close the `.pbix` file
   - Reopen it in Power BI Desktop
   - In Claude, ask again: "Can you connect to my Power BI model?"
5. **Restart both applications:**
   - Close Power BI Desktop and Claude Desktop
   - Reopen Power BI Desktop with your model
   - Reopen Claude Desktop
   - Try connecting again

### Issue: Connection Fails or Times Out

**Symptoms:**
- "Failed to connect" messages
- Timeout errors
- Connection drops unexpectedly

**Solutions:**
1. **Check Windows Firewall:**
   - Ensure Claude Desktop and Node.js are allowed through firewall
   - Add exceptions if needed
2. **Check Antivirus Software:**
   - Temporarily disable to test
   - Add exceptions for Claude Desktop and the MCP extension
3. **Verify Port Availability:**
   - Power BI Desktop uses dynamic ports
   - Ensure no other applications are interfering
4. **Review Logs:**
   - Check Claude Desktop logs for specific error messages
   - Windows: `%APPDATA%\Claude\logs`

### Issue: DAX Query Errors

**Symptoms:**
- Queries fail to execute
- "Invalid DAX syntax" errors
- "Table or column not found" errors

**Solutions:**
1. **Verify Table/Column Names:**
   - DAX is case-sensitive
   - Check exact names: "Sales" ≠ "sales"
   - Use single quotes for table names: `'Sales'`
   - Use brackets for column names: `[Amount]`
2. **Check Relationships:**
   - Ensure required relationships exist
   - Verify relationship direction
3. **Ask Claude to Explain:**
   ```
   Can you explain the DAX you're using and check if table names are correct?
   ```

### Issue: Measures Not Created

**Symptoms:**
- "Failed to create measure" errors
- Measure created but not visible
- Conflicts with existing measures

**Solutions:**
1. **Check for Name Conflicts:**
   - Measure name already exists
   - Try a different name
2. **Verify Target Table Exists:**
   - Table must exist before creating measure
   - Check table name spelling
3. **Validate DAX Expression:**
   - Ensure DAX syntax is correct
   - Test expression in Power BI Desktop first
4. **Check Permissions:**
   - Ensure model is not read-only
   - Verify you have edit permissions

### Issue: .mcpb File Won't Install

**Symptoms:**
- Installation fails
- "Invalid extension" error
- Extension doesn't appear after installation

**Solutions:**
1. **Download Again:**
   - File may be corrupted
   - Download fresh copy from GitHub
2. **Check Claude Desktop Version:**
   - Ensure you have the latest version
   - Update if necessary
3. **Try Manual Installation:**
   - Use Method 2 (manual build) instead
   - Configure via `claude_desktop_config.json`
4. **Contact Support:**
   - Check GitHub issues for similar problems
   - Create new issue if needed



## Acknowledgments

- **Maxim Anatsko** for creating the Power BI Desktop MCP extension - [GitHub Repository](https://github.com/maxanatsko/pbi-desktop-mcp-public/tree/main)
- **Anthropic** for developing Claude and the Model Context Protocol
- **Microsoft** for Power BI Desktop
- **MCP Community** for server implementations and support

## Resources

- [Power BI Desktop MCP Extension by Maxim Anatsko](https://github.com/maxanatsko/pbi-desktop-mcp-public)
- [Claude AI Documentation](https://docs.anthropic.com/)
- [Model Context Protocol](https://modelcontextprotocol.io/)
- [Power BI Documentation](https://docs.microsoft.com/power-bi/)
- [DAX Guide](https://dax.guide/)

## License

This project documentation is licensed under the MIT License - see the LICENSE file for details.

Note: The Power BI Desktop MCP extension has its own license. Please refer to the [original repository](https://github.com/maxanatsko/pbi-desktop-mcp-public) for extension licensing details.

## Contact

For questions, issues, or suggestions:
- **GitHub Issues:** Open an issue in this repository
- **LinkedIn:** [Connect with me](https://www.linkedin.com/in/your-profile)
- **Extension Issues:** Report to [Maxim's Repository](https://github.com/maxanatsko/pbi-desktop-mcp-public/issues)

---

**Happy Data Modeling with Claude and Power BI! 🚀**
