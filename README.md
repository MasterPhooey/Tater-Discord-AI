<div align="center">
  <img src="https://raw.githubusercontent.com/MasterPhooey/Tater-Discord-WebUI/refs/heads/main/images/tater.png" alt="Tater Discord Bot" width="200"/>
  <h1>Tater Discord AI & Web UI</h1>
</div>

# Tater - A Discord Bot & Web UI Powered by Ollama

Tater is a Discord bot that integrates with Ollama to provide a variety of AI-powered tools, and now it also comes with a web UI for interactive chat. Whether you're on Discord or using the web interface, Tater uses advanced memory and context retrieval with embeddings to deliver improved, continuous conversations.

<div align="center">
  <img src="https://raw.githubusercontent.com/MasterPhooey/Tater-Discord-WebUI/refs/heads/main/images/webui.png" alt="Tater Discord Bot" width="200"/>
  <h1>Tater Discord AI & Web UI</h1>
</div>


## Features

- **Conversation Continuity**: Maintains context using Redis and an embedding model for improved memory retrieval.
- **Ollama Integration**: Utilizes Ollama for AI responses, conversation memory, and embedding-based recall.
  - **Chat Responses**: Generates AI responses, waiting messages, and friendly error messages.
  - **Embedding Model**: Enhances chat history recall and provides more relevant responses by storing and retrieving past conversations.
  - **Requirements**:
    - Use an **Ollama model that supports tools** (e.g., `command-r:35b` is excellent). For more details, see [Ollama Tools](https://ollama.com/search?c=tools).
    - Use an **Ollama embedding model**. See available models here: [Ollama Embeddings](https://ollama.com/search?c=embedding).

- **Web UI Integration**: 
  - Interact with Tater via a Streamlit-based web interface.
  - Chat history, file attachments, and tool function calls are all supported.
  - Customize your user avatar and settings directly from the web UI.

- **RSS Feed Management** (Discord-only):
  - **Watch Feeds**: Add an RSS feed to the watch list.
  - **Unwatch Feeds**: Remove an RSS feed.
  - **List Feeds**: List all currently watched RSS feeds.
  - (RSS feed announcements post to a dedicated Discord channel.)

## Embedding System (Memory & Context Retrieval)

Tater uses an embedding model to store and retrieve chat context, which improves chat continuity and memory recall. Instead of relying solely on the raw chat history, Tater:

- **Generates an embedding** (a vector representation) of each message.
- **Stores embeddings in Redis** for fast and efficient retrieval.
- **Retrieves relevant past messages** when a user revisits a topic, ensuring the AI's responses are informed by context.

### **Low RAM Mode (Optional)**
- By default, the bot **stores all embeddings indefinitely**, allowing it to recall long-term conversations.
- If running on a **low-RAM system**, you can enable memory limits by modifying `embed.py`:
  ```python
  # Uncomment the following line in embed.py to limit storage to the last 100 messages (saves RAM)
  # redis_client.ltrim(global_key, -1000, -1)
  ```
  - **Uncommenting this line** will ensure only the **last 1000 embeddings** are kept in memory.
  - This helps prevent excessive memory usage on systems with limited resources.

## Available Tools

**Below are the tools available to you. Simply ask Tater to perform these tasks—no slash commands or specific key terms are required:**

- **YouTube Video Summaries:**  
  Extracts YouTube video IDs, fetches summaries, and sends formatted responses.

- **Web Summaries:**  
  Summarizes webpages or articles.

- **Image Generation:**  
  Generates images based on text prompts using Automatic111/SD.Next.

- **Premiumize.me Integration:**  
  - Checks if a given URL is cached on Premiumize.me and retrieves download links.  
  - Processes torrent files to extract the torrent hash, checks cache status, and retrieves download links.

- **RSS Feed Monitoring:**  
  Automatically monitors RSS feeds for new articles and announces summaries to RESPONSE_CHANNEL when new articles are published. This integration includes three tools:  
  - **Watch Feed:** Add an RSS feed to be monitored.  
  - **Unwatch Feed:** Remove an RSS feed from monitoring.  
  - **List Feeds:** List all currently watched RSS feeds.

- **Web Search:**  
  Searches the web for additional or up-to-date information when needed. If the AI determines that it lacks sufficient knowledge or context to answer a query, it can trigger a web search to retrieve current information and use it to generate a final, accurate answer.

## Installation

### Prerequisites
- Python 3.11
- Docker (optional, for containerized deployment)

### Setting Up Locally

1. **Clone the Repository**

```bash
git clone https://github.com/MasterPhooey/Tater.git
```

2. **Navigate to the Project Directory**

```bash
cd Tater
```

3. **Install Dependencies**

Using pip, run:

```bash
pip install -r requirements.txt
```

4. **Configure Environment Variables**

Create a `.env` file in the root directory with the following variables:

```bash
OLLAMA_HOST=127.0.0.1
OLLAMA_PORT=11434
OLLAMA_MODEL=command-r:latest
OLLAMA_EMB_MODEL=nomic-embed-text
CONTEXT_LENGTH=10000
REDIS_HOST=127.0.0.1
REDIS_PORT=6379
AUTOMATIC_URL=http://127.0.0.1:7860
PREMIUMIZE_API_KEY=your_premiumize_api_key
```

5. **Run the Web UI**

Launch the web UI using Streamlit:

```bash
streamlit run webui.py
```

### Running with Docker

1. **Build the Docker Image**

```bash
docker build -t tater .
```

2. **Run the Container**

```bash
docker run -d --name tater_bot -p 8501:8501 tater
```
