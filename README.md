# Yun - AI-Powered News Aggregation & Analysis

<div align="center">

**Intelligent News Monitoring with MCP Protocol Support**

[![License](https://img.shields.io/badge/license-GPL--3.0-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-v1.0.0-blue.svg)](https://github.com/we-dotcom-1/Yun)
[![MCP](https://img.shields.io/badge/MCP-v1.0.0-green.svg)](https://modelcontextprotocol.io/)

</div>

---

## Overview

**Yun** is a fully functional MCP (Model Context Protocol) server for intelligent news aggregation and analysis. It provides a powerful AI interface for monitoring global news trends, analyzing sentiment, and discovering emerging topics across 35+ news platforms.

### Key Features

- **MCP Protocol Support** - Seamlessly integrate with AI assistants (Claude, Cherry Studio, Cursor, etc.)
- **13 Analysis Tools** - From basic queries to advanced trend prediction
- **Multi-Platform** - Monitor 35+ news platforms (Zhihu, Weibo, Bilibili, TikTok, etc.)
- **Smart Search** - Keyword, fuzzy, and entity-based search capabilities
- **Trend Analysis** - Track topic lifecycle, detect viral content, predict future trends
- **Sentiment Analysis** - AI-powered sentiment analysis with structured prompts
- **Cross-Platform Insights** - Compare coverage across different platforms

---

## Architecture

```mermaid
graph TB
    A[AI Client] -->|MCP Protocol| B[Yun MCP Server]
    B --> C[Data Service Layer]
    C --> D[Parser Service]
    C --> E[Cache Service]
    B --> F[Tool Modules]
    F --> G[Data Query Tools]
    F --> H[Analytics Tools]
    F --> I[Search Tools]
    F --> J[System Management]
    D --> K[News Data Sources]
    K --> L[Zhihu]
    K --> M[Weibo]
    K --> N[Bilibili]
    K --> O[35+ Platforms]
    
    style A fill:#e3f2fd
    style B fill:#fff3e0
    style F fill:#e8f5e9
    style K fill:#f3e5f5
```

---

## Quick Start

### Prerequisites

- **Python 3.10+**
- **UV** (Universal Virtualenv) - [Installation Guide](https://github.com/astral-sh/uv)

### Installation

#### Windows

```batch
# Run the setup script
setup-windows.bat
```

#### Mac/Linux

```bash
# Make setup script executable
chmod +x setup-mac.sh

# Run setup
./setup-mac.sh
```

### Start the MCP Server

#### STDIO Mode (for AI Clients)

```bash
uv run python -m mcp_server.server
```

#### HTTP Mode (for Remote Access)

```batch
# Windows
start-http.bat

# Mac/Linux
./start-http.sh
```

The HTTP server will start at `http://localhost:3333/mcp`

---

## Connect to AI Clients

### Cherry Studio (Recommended for Beginners)

1. Open Cherry Studio
2. Go to **Settings** > **MCP Servers**
3. Add new server with these settings:
   - **Name**: Yun
   - **Type**: STDIO
   - **Command**: [Path to UV]
   - **Args**: `--directory [Path to Yun] run python -m mcp_server.server`

### Claude Desktop

Edit your config file (`~/Library/Application Support/Claude/claude_desktop_config.json` on Mac):

```json
{
  "mcpServers": {
    "yun": {
      "command": "uv",
      "args": [
        "--directory",
        "/path/to/Yun",
        "run",
        "python",
        "-m",
        "mcp_server.server"
      ]
    }
  }
}
```

### Cursor

Create `.cursor/mcp.json` in your project:

```json
{
  "mcpServers": {
    "yun": {
      "url": "http://localhost:3333/mcp",
      "description": "Yun News Aggregation and Analysis"
    }
  }
}
```

---

## Tool Categories & Workflow

```mermaid
graph LR
    A[User Query] --> B{Tool Selection}
    B -->|Basic Data| C[Data Query Tools]
    B -->|Search| D[Search Tools]
    B -->|Analysis| E[Analytics Tools]
    B -->|System| F[System Tools]
    
    C --> G[get_latest_news]
    C --> H[get_news_by_date]
    C --> I[get_trending_topics]
    
    D --> J[search_news]
    D --> K[search_related_news_history]
    
    E --> L[analyze_topic_trend]
    E --> M[analyze_sentiment]
    E --> N[analyze_data_insights]
    E --> O[find_similar_news]
    E --> P[generate_summary_report]
    
    F --> Q[get_current_config]
    F --> R[get_system_status]
    F --> S[trigger_crawl]
    
    G --> T[Results]
    H --> T
    I --> T
    J --> T
    K --> T
    L --> T
    M --> T
    N --> T
    O --> T
    P --> T
    Q --> T
    R --> T
    S --> T
    
    style A fill:#e3f2fd
    style C fill:#fff3e0
    style D fill:#e8f5e9
    style E fill:#f3e5f5
    style F fill:#fce4ec
    style T fill:#c8e6c9
```

---

## Available Tools

### Basic Data Query

| Tool | Description | Use Case |
|------|-------------|----------|
| `get_latest_news` | Get the most recent news | Quick overview of current events |
| `get_news_by_date` | Query news by date (natural language) | Historical analysis, date-specific queries |
| `get_trending_topics` | Get trending topics from watchlist | Monitor personal interests |

### Intelligent Search

| Tool | Description | Use Case |
|------|-------------|----------|
| `search_news` | Unified search (keyword/fuzzy/entity) | Find specific topics or content |
| `search_related_news_history` | Find related historical news | Discover topic evolution |

### Advanced Analysis

| Tool | Description | Use Case |
|------|-------------|----------|
| `analyze_topic_trend` | Topic trend analysis | Track heat, lifecycle, viral detection |
| `analyze_data_insights` | Platform comparison & activity stats | Cross-platform analysis |
| `analyze_sentiment` | Sentiment analysis | Public opinion tracking |
| `find_similar_news` | Find similar articles | Content deduplication |
| `generate_summary_report` | Daily/weekly summaries | Periodic reporting |

### System Management

| Tool | Description | Use Case |
|------|-------------|----------|
| `get_current_config` | Get current configuration | System inspection |
| `get_system_status` | System health check | Monitoring |
| `trigger_crawl` | Manually trigger crawling | On-demand updates |

---

## Data Flow

```mermaid
sequenceDiagram
    participant User
    participant AI
    participant YunMCP
    participant DataService
    participant Parser
    participant DataSource
    
    User->>AI: Natural language query
    AI->>YunMCP: MCP tool call
    YunMCP->>DataService: Request data
    
    alt Data in cache
        DataService-->>YunMCP: Return cached data
    else Data not cached
        DataService->>Parser: Parse request
        Parser->>DataSource: Fetch news data
        DataSource-->>Parser: Raw data
        Parser-->>DataService: Processed data
        DataService->>DataService: Cache result
        DataService-->>YunMCP: Return data
    end
    
    YunMCP->>YunMCP: Format response
    YunMCP-->>AI: Structured JSON
    AI-->>User: Natural language response
```

---

## Usage Examples

### Query Today's News

```
"Show me the top 10 news from today"
```

### Search Specific Topics

```
"Search for news about 'artificial intelligence' in the last 7 days"
```

### Trend Analysis

```
"Analyze the trend of 'Tesla' over the past week"
```

### Sentiment Analysis

```
"Analyze the sentiment of news about 'Bitcoin' from yesterday"
```

### Platform Comparison

```
"Compare how different platforms cover 'iPhone' news"
```

---

## Project Structure

```
Yun/
├── mcp_server/           # MCP Server implementation
│   ├── server.py         # Main server entry
│   ├── tools/            # MCP tool implementations
│   │   ├── data_query.py       # Data query tools
│   │   ├── analytics.py        # Advanced analytics
│   │   ├── search_tools.py     # Search functionality
│   │   ├── config_mgmt.py      # Configuration management
│   │   └── system.py           # System management
│   ├── services/         # Core services
│   │   ├── data_service.py     # Data access layer
│   │   ├── parser_service.py   # Data parsing
│   │   └── cache_service.py    # Caching layer
│   └── utils/            # Utilities
│       ├── date_parser.py      # Date parsing
│       ├── validators.py       # Input validation
│       └── errors.py           # Error handling
├── config/               # Configuration files
│   ├── config.yaml       # Main configuration
│   └── frequency_words.txt # Keyword watchlist
├── pyproject.toml        # Project metadata
├── requirements.txt      # Python dependencies
└── README.md             # This file
```

---

## Configuration

### Main Configuration (`config/config.yaml`)

Key settings:
- **Crawler settings** - Enable/disable crawling, proxy settings
- **Report mode** - Daily, current, or incremental
- **Platform list** - Which platforms to monitor
- **Weight algorithm** - Customize news ranking

### Keyword Watchlist (`config/frequency_words.txt`)

Add keywords you want to monitor:
- **Normal keywords** - Basic matching
- **Required keywords** (`+word`) - Must appear in title
- **Filter keywords** (`!word`) - Exclude from results

Example:
```
AI
ChatGPT
+technology
!advertisement
```

---

## System Architecture Details

```mermaid
graph TB
    subgraph "MCP Server Core"
        A[FastMCP Application]
        B[Tool Registry]
        C[Request Handler]
    end
    
    subgraph "Service Layer"
        D[Data Service]
        E[Parser Service]
        F[Cache Service]
    end
    
    subgraph "Tool Modules"
        G[Data Query]
        H[Analytics]
        I[Search]
        J[Config]
        K[System]
    end
    
    subgraph "Utilities"
        L[Validators]
        M[Date Parser]
        N[Error Handler]
    end
    
    A --> B
    B --> C
    C --> G
    C --> H
    C --> I
    C --> J
    C --> K
    
    G --> D
    H --> D
    I --> D
    J --> D
    K --> D
    
    D --> E
    D --> F
    
    G --> L
    H --> L
    I --> L
    G --> M
    H --> M
    I --> M
    
    style A fill:#e3f2fd
    style D fill:#fff3e0
    style G fill:#e8f5e9
    style L fill:#f3e5f5
```

---

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### Development Setup

1. Clone the repository
2. Run setup script
3. Make your changes
4. Test thoroughly
5. Submit PR

---

## License

This project is licensed under the GPL-3.0 License - see the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

- Built with [FastMCP](https://github.com/jlowin/fastmcp)
- Data sourced from [NewsNow](https://github.com/ourongxing/newsnow)
- MCP Protocol by [Anthropic](https://modelcontextprotocol.io/)

---

## Support

- **GitHub Issues**: For bug reports and feature requests
- **Discussions**: For questions and community support

---

<div align="center">

**Made for the AI Community**

[Star this repo](https://github.com/we-dotcom-1/Yun) if you find it useful!

</div>
