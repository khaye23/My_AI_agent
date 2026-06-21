AI Search Agent

A local AI-powered research assistant built using LangChain, Ollama, and Llama 3.

This project demonstrates how to create a ReAct-style AI agent capable of:

reasoning
tool usage
internet searching
Wikipedia lookup
mathematical calculations
conversational memory
Streamlit deployment

The agent runs locally using Ollama and Llama 3.

Features
Local LLM using Ollama + Llama3
LangChain ReAct Agent
Wikipedia search tool
DuckDuckGo internet search
Calculator tool
Conversation memory
Streamlit web app (experimental setup)
Google Colab compatible
Tech Stack
Python
LangChain
Ollama
Llama3
Streamlit
DuckDuckGo Search
Wikipedia API
Google Colab
Architecture
User Question
      ↓
ReAct Agent
      ↓
Llama3 (reasoning)
      ↓
Tools
 ├── Wikipedia
 ├── DuckDuckGo Search
 └── Calculator
      ↓
Final Answer
Installation
1. Install Ollama
curl -fsSL https://ollama.com/install.sh | sh
2. Start Ollama
ollama serve
3. Pull Llama3
ollama pull llama3
4. Install Python Dependencies
pip install -U \
langchain==0.2.16 \
langchain-community==0.2.16 \
langchain-core==0.2.38 \
langchain-ollama \
wikipedia \
python-dotenv \
pydantic \
duckduckgo-search \
arxiv \
streamlit
Project Structure
project/
│
├── AIsearchAgent.py
├── app.py
├── README.md
└── requirements.txt
LLM Setup
from langchain_ollama import ChatOllama

llm = ChatOllama(
    model="llama3",
    temperature=0
)
Memory Setup
from langchain.memory import ConversationBufferMemory

memory = ConversationBufferMemory(
    memory_key="chat_history",
    return_messages=True
)
Tools
Wikipedia Tool
wiki_api = WikipediaAPIWrapper()

wiki_tool = WikipediaQueryRun(
    api_wrapper=wiki_api
)
DuckDuckGo Search Tool
search = DuckDuckGoSearchRun()
Calculator Tool
def calculator(expression):
    return eval(expression)
ReAct Agent
prompt = hub.pull("hwchase17/react")

agent = create_react_agent(
    llm=llm,
    tools=tools,
    prompt=prompt
)
Agent Executor
agent_executor = AgentExecutor(
    agent=agent,
    tools=tools,
    memory=memory,
    verbose=False,
    handle_parsing_errors=True
)
Example Usage
response = agent_executor.invoke({
    "input": "What is the capital of South Africa?"
})

print(response)
Example Output
{
    'input': 'What is the capital of South Africa?',
    'output': 'The capital of South Africa is Pretoria.'
}
Running Streamlit
streamlit run app.py
Deployment Note (Streamlit Issue)

Streamlit was attempted for the web interface, but installation was unstable on my system due to large dependency requirements and compatibility issues with some libraries (especially langchain-community and related packages).

As a result:

The Streamlit app may not run reliably in all environments
Dependency installation issues were encountered during setup
However, the AI agent itself works correctly in a Jupyter notebook environment without issues

So the core functionality of the project (ReAct agent + tools + reasoning) runs successfully, even though the Streamlit UI layer was not fully stable on my device.

Common Issues
JSONDecodeError

Cause:
Wikipedia API returned invalid or empty JSON.

Fix:

handle_parsing_errors=True
OutputParserException

Cause:
The LLM returned invalid ReAct formatting.

Fix:
Use the official ReAct prompt:

prompt = hub.pull("hwchase17/react")
Ollama Connection Error
could not connect to ollama server

Fix:

ollama serve
Future Improvements
Add Arxiv research tool
Add PDF document Q&A
Add vector database memory
Add multi-agent workflows
Add voice assistant support
Add autonomous planning
Author

Built with LangChain + Ollama + Llama 3