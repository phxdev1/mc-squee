# mc-squee

A zero-infrastructure Multi-Agent Communication Protocol (MCP) server powered by GitHub Actions.

## 🚀 Overview

mc-squee is a novel approach to hosting Multi-Agent Communication Protocols without deploying any infrastructure. It leverages GitHub Actions as a runtime environment and uses the repository itself as the data store, allowing you to instantly deploy and host MCPs with a simple workflow trigger.

## ✨ Features

- **Zero Infrastructure**: No servers, databases, or cloud providers required
- **One-Click Deployment**: Deploy any MCP protocol with a single workflow trigger
- **Git-Based Storage**: All protocol data, agent registrations, and messages stored directly in Git
- **Multi-Tenant**: Host multiple MCP protocols in the same repository
- **Fully Versioned**: Complete history of all MCP activity tracked in Git
- **Free to Run**: Uses only GitHub's free tier services

## 🛠️ How It Works

mc-squee repurposes GitHub's built-in functionality to create a complete MCP hosting environment:

1. **GitHub Actions** serves as the runtime environment
2. **Repository Dispatch** functions as the API endpoint
3. **Git Repository** acts as the persistent storage
4. **GitHub Pages** provides API documentation

## 🏁 Getting Started

### Prerequisites

- A GitHub repository
- GitHub Actions enabled

### Installation

1. Create a `.github/workflows` directory in your repository (if it doesn't exist)
2. Add the `mcp-server.yml` workflow file to this directory
3. Commit and push these changes

### Deploying Your First MCP

1. Navigate to the "Actions" tab in your repository
2. Select the "Zero-Infrastructure MCP Server" workflow
3. Click "Run workflow"
4. Fill in the required parameters:
   - **Protocol Repository URL**: The GitHub repository URL containing the MCP protocol
   - **Protocol Branch**: The branch of the protocol to use (default: main)
   - **Tenant Name**: A unique name for this protocol deployment
5. Click "Run workflow" to deploy

Once deployed, the workflow will:
1. Clone and validate the protocol repository
2. Register the protocol in the system
3. Generate API credentials for interacting with the protocol
4. Create documentation with usage examples
5. Upload API credentials as a workflow artifact for you to download

## 📝 Using Your MCP

### Registering an Agent

```bash
curl -X POST \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "event_type": "mcp_message",
    "client_payload": {
      "action": "register_agent",
      "protocol_id": "YOUR_PROTOCOL_ID",
      "agent_id": "your-agent-id",
      "agent_name": "Your Agent Name",
      "capabilities": ["capability1", "capability2"],
      "webhook_url": "https://your-agent-service.com/webhook"
    }
  }' \
  https://api.github.com/repos/YOUR_USERNAME/YOUR_REPO/dispatches
```

### Sending a Message

```bash
curl -X POST \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "event_type": "mcp_message",
    "client_payload": {
      "action": "send_message",
      "protocol_id": "YOUR_PROTOCOL_ID",
      "message": {
        "from_agent": "agent-a",
        "to_agent": "agent-b",
        "type": "request",
        "content": {
          "your_message": "data_here"
        }
      }
    }
  }' \
  https://api.github.com/repos/YOUR_USERNAME/YOUR_REPO/dispatches
```

## 🏗️ Protocol Structure

To be compatible with mc-squee, a protocol repository should have the following structure:

```
protocol-repo/
├── protocol.json (or protocol.yaml, protocol.yml)
├── schemas/
│   ├── messages.json
│   └── ...
└── handlers/
    ├── request.py
    └── ...
```

At minimum, the protocol repository must contain:
- A protocol configuration file (protocol.json, protocol.yaml, or protocol.yml)
- A schemas directory with message definitions

## 📊 System Architecture

```
┌─────────────────┐      ┌───────────────────┐
│                 │      │                   │
│  GitHub Actions │◄────►│ Git Repository    │
│  (Runtime)      │      │ (Storage)         │
│                 │      │                   │
└────────┬────────┘      └───────────────────┘
         │
         │
         ▼
┌─────────────────┐      ┌───────────────────┐
│                 │      │                   │
│  Repository     │◄────►│ Agent Services    │
│  Dispatch (API) │      │                   │
│                 │      │                   │
└─────────────────┘      └───────────────────┘
```

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## ⚠️ Limitations

- GitHub Actions have usage limits which may affect high-volume MCP deployments
- Webhook delivery is not guaranteed and lacks retry mechanisms
- Repository size may grow over time as message history accumulates
- Not suitable for real-time communication requiring low latency

## 🙏 Acknowledgments

- GitHub for providing the Actions infrastructure that makes this possible
- The AI agent research community for advancing Multi-Agent Communication Protocols
