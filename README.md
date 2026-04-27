# 🧠 AI Agent with Memory (AWS AgentCore)

This project demonstrates a production-style AI agent built using Amazon Bedrock and AgentCore with persistent memory and tool usage.

---

## 🚀 Features

- Stateful AI agent (remembers user facts and preferences)
- Integration with Amazon Bedrock
- Tool usage (calculator)
- Session-based conversations
- Multi-user support (actor_id)

---

## 🏗 Architecture

Client → AgentCore Runtime → Agent → Bedrock Model  
                               ↓  
                             Memory  

# 1. Create project and install dependencies
mkdir agentcore-tutorial && cd agentcore-tutorial
uv init --no-workspace && uv add bedrock-agentcore-starter-toolkit

# 2. Create a deployment folder and add the pyproject.toml file needed:
mkdir agent_deployment
uv init --bare ./agent_deployment && uv --directory ./agent_deployment add strands-agents bedrock-agentcore strands-agents-tools

# 3. Save the agent code above in to the agent_deployment folder as tutorial_agent.py

# 4. Configure and deploy
uv run agentcore configure -e ./agent_deployment/tutorial_agent.py

uv run agentcore launch

# 5. Test your deployed agent
uv run agentcore invoke '{"prompt": "What is 25 * 4 + 10?"}'

# Session 1: Store user preferences
uv run agentcore invoke '{"prompt": "Remember that I absolutely love hot tea, especially Earl Grey, and I prefer it with a splash of milk and one sugar"}' --session-id 12345-12345-12345-12345-12345-12345-A --headers "Actor-Id:user123"

# Session 2: Different session, retrieve the preferences
uv run agentcore invoke '{"prompt": "What kind of hot drinks do I like and how do I prefer them prepared?"}' --session-id 12345-12345-12345-12345-12345-12345-B --headers "Actor-Id:user123"

