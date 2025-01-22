# DeepSeek API Proxy

A high-performance HTTP/2-enabled proxy server designed specifically to enable Cursor IDE's Composer to use DeepSeek's language models. This proxy translates OpenAI-compatible API requests to DeepSeek API format, allowing Cursor's Composer and other OpenAI API-compatible tools to seamlessly work with DeepSeek's models.

## Primary Use Case

This proxy was created to enable Cursor IDE users to leverage DeepSeek's powerful language models through Cursor's Composer interface as an alternative to OpenAI's models. By running this proxy locally, you can configure Cursor's Composer to use DeepSeek's models for AI assistance, code generation, and other AI features. It handles all the necessary request/response translations and format conversions to make the integration seamless.

## Features

- HTTP/2 support for improved performance
- Full CORS support
- Streaming responses
- Support for function calling/tools
- Automatic message format conversion
- Compression support (Brotli, Gzip, Deflate)
- Compatible with OpenAI API client libraries

## Prerequisites

- Go 1.19 or higher
- DeepSeek API key
- Public Endpoint

## Installation

1. Clone the repository
2. Install dependencies:
```bash
go mod download
```

## Configuration

1. Create a `.env` file in the project root:
```bash
DEEPSEEK_API_KEY=your_deepseek_api_key_here
```

## Usage

1. Start the proxy server:
```bash
go run proxy.go
```

The server will start on port 9000 by default.

2. Use the proxy with your OpenAI API clients by setting the base URL to `http://your-public-endpoint:9000`

### Supported Endpoints

- `/v1/chat/completions` - Chat completions endpoint
- `/v1/models` - Models listing endpoint

### Model Mapping

- `gpt-4o` maps to DeepSeek's GPT-4o equivalent model
- `deepseek-chat` for DeepSeek's native chat model

## Dependencies

- `github.com/andybalholm/brotli` - Brotli compression support
- `github.com/joho/godotenv` - Environment variable management
- `golang.org/x/net` - HTTP/2 support

## Security

- The proxy includes CORS headers for cross-origin requests
- API keys are required and validated
- Secure handling of request/response data

## License

This project is licensed under the GNU General Public License v2.0 (GPLv2). See the [LICENSE.md](LICENSE.md) file for details. 