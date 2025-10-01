# AI Chatbot with Memory & Web Search

A sophisticated conversational AI chatbot built with LangGraph, featuring persistent memory, intelligent tool selection, and real-time web search capabilities. The chatbot dynamically decides when to search the web for the latest information or use Wikipedia for encyclopedic knowledge.

## Overview

This project demonstrates an advanced AI agent implementation using LangGraph's state management system. The chatbot maintains conversation history across sessions and intelligently routes queries to appropriate tools based on context.

## Key Features

- **Intelligent Tool Routing**: Automatically selects the right tool (Wikipedia, DuckDuckGo search, or custom functions) based on user queries
- **Persistent Memory**: Maintains conversation context using LangGraph's MemorySaver for natural, multi-turn conversations
- **Real-time Web Search**: Fetches the latest information from the web when needed
- **Wikipedia Integration**: Quick access to encyclopedic knowledge
- **Thread-based Sessions**: Supports multiple concurrent users with unique conversation threads
- **State Management**: Built-in checkpoint system saves conversation state at each step

## Architecture

The chatbot uses a graph-based architecture with the following components:

- **Chat Node**: Processes user input and determines if tools are needed
- **Tool Node**: Executes selected tools (Wikipedia, web search, custom functions)
- **Conditional Routing**: Dynamically routes between chat and tool nodes based on LLM decisions
- **Memory Layer**: Stores conversation history in RAM for the session lifetime

## Technologies Used

- **LangGraph**: State graph framework for building stateful AI agents
- **LangChain**: Framework for LLM application development
- **OpenAI GPT-4o-mini**: Language model for natural conversation
- **DuckDuckGo Search**: Web search capabilities
- **Wikipedia API**: Encyclopedia knowledge access
- **FAISS**: Vector store for document retrieval
- **Python 3.x**: Core programming language

## Prerequisites

- Python 3.8 or higher
- OpenAI API key
- Required Python packages (see Installation)

## Installation

1. Clone the repository:
```bash
git clone <your-repo-url>
cd <project-directory>
```

2. Create a virtual environment:
```bash
python -m venv myenv
source myenv/bin/activate  # On Windows: myenv\Scripts\activate
```

3. Install required packages:
```bash
pip install langgraph langchain langchain-openai langchain-community
pip install python-dotenv pydantic
pip install duckduckgo-search wikipedia-api
pip install faiss-cpu youtube-transcript-api
pip install arxiv pymupdf requests
```

4. Set up environment variables:
Create a `.env` file in the project root:
```env
OPENAI_API_KEY=your_openai_api_key_here
```

## Usage

### Running the Chatbot

Open the Jupyter notebook `Chatbot_LG.ipynb` and run the cells sequentially. The main chatbot loop is:

```python
thread_id = "1"  # Unique ID for each user session

while True:
    user_query = input("User: ")

    if user_query.strip().lower() in ['exit', 'bye', 'quit']:
        break

    config = {'configurable': {'thread_id': thread_id}}

    response = chatbot.invoke({
        'messages': [HumanMessage(content=user_query)]
    }, config=config)

    print(f"AI: {response['messages'][-1].content}")
```

### Example Conversations

**Example 1: Web Search**
```
User: recent news on AI
AI: [Searches the web and provides latest AI news]
```

**Example 2: General Knowledge**
```
User: Who invented the telephone?
AI: [Uses Wikipedia to provide accurate historical information]
```

**Example 3: Custom Functions**
```
User: Add 25 and 47
AI: 72
```

## How It Works

### State Management

The chatbot uses a typed state dictionary to manage conversation flow:

```python
class ChatState(TypedDict):
    messages: Annotated[list[BaseMessage], add_messages]
```

### Tool Integration

Three main tools are available:
1. **add_numbers**: Basic arithmetic operations
2. **Wikipedia**: Encyclopedic knowledge lookup
3. **DuckDuckGo Search**: Real-time web search

The LLM automatically decides which tool to use based on the user's query.

### Memory System

Memory is implemented through:
- **MemorySaver**: Stores conversation state in RAM
- **Thread ID**: Unique identifier for each conversation session
- **Checkpoints**: Automatic state snapshots at each step

### Graph Architecture

```
START → Chat Node → [Conditional Edge]
                      ↓
                  Tool Node → Chat Node → END
```

## Advanced Features

### Retrieving Conversation History

```python
# Get current state
current_state = chatbot.get_state(config)

# Get full conversation history
history = list(chatbot.get_state_history(config))
```

### Multiple User Sessions

Each user can have their own conversation thread:

```python
user1_config = {'configurable': {'thread_id': 'user1'}}
user2_config = {'configurable': {'thread_id': 'user2'}}
```

## Project Structure

```
.
├── Chatbot_LG.ipynb      # Main notebook with chatbot implementation
├── .env                   # Environment variables (API keys)
└── README.md              # Project documentation
```

## Limitations

- Memory is stored in RAM and cleared when the program stops
- DuckDuckGo search may have rate limits
- Requires active internet connection for web search and Wikipedia
- OpenAI API usage incurs costs

## Future Enhancements

- Persistent memory using database storage
- Voice input/output capabilities
- Multi-modal support (images, documents)
- Custom document retrieval with vector stores
- Streaming responses for real-time output
- Web interface for easier interaction

## Troubleshooting

**Issue**: `duckduckgo_search` package renamed warning
- **Solution**: Install the updated package: `pip install ddgs`

**Issue**: OpenAI API authentication errors
- **Solution**: Verify your API key in the `.env` file

**Issue**: Memory not persisting across sessions
- **Solution**: This is expected behavior - memory is RAM-based and clears when the program stops

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## License

This project is open source and available under the MIT License.

## Acknowledgments

- Built with [LangGraph](https://github.com/langchain-ai/langgraph)
- Powered by [LangChain](https://github.com/langchain-ai/langchain)
- Uses [OpenAI's GPT models](https://openai.com/)

## Contact

For questions or feedback, please open an issue in the repository.

---

**Note**: This is an educational project demonstrating LangGraph's capabilities for building stateful AI agents with memory and tool integration.