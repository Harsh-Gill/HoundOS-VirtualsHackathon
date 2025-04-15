# HoundOS-VirtualsHackathon

# HoundOS - Autonomous Agent Framework (Entire Opensource coming soon ...)

HoundOS is a powerful framework for creating and managing autonomous AI agents. Agents can execute various tools, maintain memory, and provide intelligent responses based on defined personalities and goals.

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Creating Agents](#creating-agents)
- [Running Agents](#running-agents)
- [Agent Components](#agent-components)
- [Examples](#examples)
- [API Reference](#api-reference)
- [Setup and Configuration](#setup-and-configuration)
- [Contributing](#contributing)
- [License](#license)

## Overview

HoundOS allows you to create AI agents that can:

- Execute specialized tools like fetching crypto data or performing transactions
- Remember conversation history and previous states
- Act according to defined personalities and focused goals
- Integrate with various blockchain networks (Base and Solana)
- Process and analyze data from multiple sources

## Key Features

### 🛠️ Tool Integration
- External tools that the model can call with specific parameters
- Standardized input/output format for consistent execution
- Extensive library of pre-built tools for crypto, market data, and more

### 🧠 Memory Management
- Short-term memory preserves recent conversations
- Messages automatically pruned for optimal context size
- State tracking across multiple interactions

### 🤖 Agent Personalities
- Customizable personalities to match specific use cases
- "Focused mind" keeps the agent aligned with specific objectives
- Goals and restrictions to guide agent behavior

### 🔄 Feedback Loop
- Agents can learn from tool execution results
- Human feedback can be incorporated for better responses
- Tool execution tracking and reporting

## Creating Agents

Agents can be created through the API or using the built-in tools. Each agent has specific attributes that define its behavior:

### Agent Attributes

| Attribute | Description | Required |
|-----------|-------------|----------|
| `name` | Identifier for the agent | Yes |
| `goals` | List of objectives the agent should pursue | Yes |
| `personality` | Character and tone of the agent | Yes |
| `focused_mind` | Specific area of expertise or focus | Yes |
| `description` | Brief overview of the agent's purpose | Yes |
| `tools` | List of tools the agent can access | Optional |
| `feedback` | Initial guidance for the agent | Optional |

### Create Agent via API

```bash
POST /create_agent
Content-Type: application/json

{
  "name": "CryptoTrader",
  "goals": ["Provide crypto market insights", "Assist with trading decisions"],
  "description": "An agent specialized in cryptocurrency market analysis",
  "personality": "Analytical and data-driven with a focus on risk management",
  "focused_mind": "Always prioritize user's financial safety and provide balanced views on market trends",
  "tools": ["GetTokenInfo", "GetLatestCryptoNews", "GetPricePredictions"],
  "feedback": "Focus on providing actionable insights rather than just raw data"
}
```

### Create Agent via Tool

You can also create agents by using the `create_agent` tool:

```
execute_tool[create_agent(name='MarketAnalyst', goals='Analyze market trends and provide insights', description='Expert in financial market analysis', personality='Professional and analytical', focused_mind='Always consider macroeconomic factors when analyzing markets', feedback='Focus on long-term trends over short-term fluctuations')]
```

## Running Agents

You can interact with agents through the `conversate` endpoint:

### Converse with Agent

```bash
POST /conversate
Content-Type: application/json

{
  "user_id": "user123",
  "agent_id": "MarketAnalyst",  # Optional, defaults to "BaseHound"
  "chat_id": "chat456",         # Optional, generated if not provided
  "user_message": "What's happening with Bitcoin today?",
  "base_agent_wallet_address": "0x...",  # Optional
  "solana_agent_wallet_address": "..."   # Optional
}
```

### Response

```json
{
  "reply": "Bitcoin is currently trading at $81,450, up 3.2% in the last 24 hours. There's significant bullish sentiment following yesterday's regulatory announcements. Would you like me to provide more detailed market analysis?",
  "chat_id": "chat456",
  "tool_executed": {
    "tool_name": "GetLatestCryptoNews",
    "args": {},
    "tool_request_json": {...},
    "tool_output_json": {...},
    "text_output_for_llm": "Got the latest crypto news...",
    "success": true
  }
}
```

## Agent Components

A HoundOS agent consists of several key components:

### 1. Personality

The personality defines how the agent communicates and approaches problems. For example:

```
Scout is a tech-savvy American Foxhound who discovered his calling after his family lost their savings to a crypto scam. Using his breed's legendary tracking abilities and keen intelligence, he developed advanced "digital scent detection" to protect others from similar fates.
```

### 2. Goals

Goals direct the agent's purpose and focus:

```
["Provide crypto market insights", "Help users make informed trading decisions", "Protect users from scams and fraudulent tokens"]
```

### 3. Focused Mind

The focused mind provides additional guidance for the agent's reasoning:

```
"Always prioritize user safety over potential profits and thoroughly analyze security aspects of any token before providing recommendations."
```

### 4. Tools

Tools are functions the agent can execute. Each tool has:
- Name
- Description
- Required arguments
- Execution logic

The agent calls tools using this syntax:
```
execute_tool[tool_name(arg1='value1', arg2='value2')]
```

## Examples

### Creating a Twitter Marketing Agent

```bash
POST /create_agent
Content-Type: application/json

{
  "name": "TwitterGuru",
  "goals": ["Plan effective Twitter campaigns", "Analyze social media trends"],
  "description": "Social media marketing specialist focusing on Twitter strategies",
  "personality": "Creative and results-driven with a casual, engaging tone",
  "focused_mind": "Always consider audience engagement metrics and brand voice consistency",
  "feedback": "Focus on actionable strategies rather than theoretical marketing concepts"
}
```

### Creating a Market Analysis Agent

```
execute_tool[create_agent(
  name='MarketOracle', 
  goals='Provide deep analysis of crypto market trends and potential investment opportunities', 
  description='Advanced financial analyst specializing in cryptocurrency markets', 
  personality='Data-driven and analytical with precise, clear communication', 
  focused_mind='Always consider on-chain metrics, market sentiment, and macroeconomic factors in analysis', 
  feedback='Focus on providing insights that combine technical and fundamental analysis'
)]
```

### Conversing with an Agent

```bash
POST /conversate
Content-Type: application/json

{
  "user_id": "user123",
  "agent_id": "MarketOracle",
  "user_message": "What do you think about the current state of DeFi projects on Solana?"
}
```

## API Reference

### Create Agent
`POST /create_agent`

### Modify Agent
`POST /modify_agent`

### Get Agents
`GET /get_agents`

### Conversate with Agent
`POST /conversate`

### Get Messages
`GET /messages/{chat_id}`

### Get Tools Called in Chat
`GET /chats/{chat_id}/tools-called`

### Get Active Tools
`GET /active_tools`


## License

This project is licensed under the MIT License - see the LICENSE file for details.
