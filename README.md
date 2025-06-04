# DeepSeek API Proxy

A high-performance HTTP/2-enabled proxy server designed specifically to enable Cursor IDE's Composer to use DeepSeek's, OpenRouter's and Ollama's language models. This proxy translates OpenAI-compatible API requests to DeepSeek/OpenRouter/Ollama API format, allowing Cursor's Composer and other OpenAI API-compatible tools to seamlessly work with these models.

## Primary Use Case

This proxy was created to enable Cursor IDE users to leverage DeepSeek's, OpenRouter's and Ollama's powerful language models through Cursor's Composer interface as an alternative to OpenAI's models. By running this proxy locally, you can configure Cursor's Composer to use these models for AI assistance, code generation, and other AI features. It handles all the necessary request/response translations and format conversions to make the integration seamless.

## Features

- HTTP/2 support for improved performance
- Full CORS support
- Streaming responses
- Support for function calling/tools
- Automatic message format conversion
- Compression support (Brotli, Gzip, Deflate)
- Compatible with OpenAI API client libraries
- API key validation for secure access
- Docker container support with multi-variant builds

## Prerequisites

- Cursor Pro Subscription
- Go 1.19 or higher
- DeepSeek or OpenRouter API key
- Ollama server running locally (optional, for Ollama support)
- Public Endpoint

## Installation

1. Clone the repository
2. Install dependencies:
```bash
go mod download
```

### Docker Installation

The proxy supports both DeepSeek and OpenRouter variants. Choose the appropriate build command for your needs:

1. Build the Docker image:
   - For DeepSeek (default):
   ```bash
   docker build -t cursor-deepseek .
   ```
   - For OpenRouter:
   ```bash
   docker build -t cursor-openrouter --build-arg PROXY_VARIANT=openrouter .
   ```
   - For Ollama:
   ```bash
   docker build -t cursor-ollama --build-arg PROXY_VARIANT=ollama .
   ```

2. Configure environment variables:
   - Copy the example configuration:
   ```bash
   cp .env.example .env
   ```
   - Edit `.env` and add your API key (either DeepSeek or OpenRouter)

3. Run the container:
```bash
docker run -p 9000:9000 --env-file .env cursor-deepseek
# OR for OpenRouter
docker run -p 9000:9000 --env-file .env cursor-openrouter
# OR for Ollama
docker run -p 9000:9000 --env-file .env cursor-ollama
```

## Configuration

The repository includes an `.env.example` file showing the required environment variables. To configure:

1. Copy the example configuration:
```bash
cp .env.example .env
```

2. Edit `.env` and add your API key:
```bash
# For DeepSeek
DEEPSEEK_API_KEY=your_deepseek_api_key_here

# OR for OpenRouter
OPENROUTER_API_KEY=your_openrouter_api_key_here

# OR for Ollama
OLLAMA_API_KEY=your_ollama_api_key_here
```

Note: Only configure ONE of the API keys based on which variant you're using.

## Usage

1. Start the proxy server:
```bash
go run proxy.go
# OR you can specify a model:
go run proxy.go -model coder
# OR
go run proxy.go -model chat
# OR for OpenRouter
go run proxy-openrouter.go
# OR for Ollama
go run proxy-ollama.go
```

The server will start on port 9000 by default.

2. Use the proxy with your OpenAI API clients by setting the base URL to `http://your-public-endpoint:9000/v1`


## Exposing the Endpoint Publicly

You can expose your local proxy server to the internet using ngrok or similar services. This is useful when you need to access the proxy from external applications or different networks.

### Using ngrok

1. Install ngrok from https://ngrok.com/download

2. Start your proxy server locally (it will run on port 9000)

3. In a new terminal, run ngrok:
```bash
ngrok http 9000
```

4. ngrok will provide you with a public URL (e.g., https://your-unique-id.ngrok.io)

5. Use this URL as your OpenAI API base URL in Cursor's settings:
```
https://your-unique-id.ngrok.io/v1
```

### Alternative Methods

You can also use other services to expose your endpoint:

1. **Cloudflare Tunnel**: 
   - Install cloudflared
   - Run: `cloudflared tunnel --url http://localhost:9000`

2. **LocalTunnel**:
   - Install: `npm install -g localtunnel`
   - Run: `lt --port 9000`

Remember to always secure your endpoint appropriately when exposing it to the internet.


### Supported Endpoints

- `/v1/chat/completions` - Chat completions endpoint
- `/v1/models` - Models listing endpoint

### Model Mapping

- `gpt-4o` maps to DeepSeek's GPT-4o equivalent model
- `deepseek-chat` for DeepSeek's native chat model
- `deepseek/deepseek-chat` for OpenRouter's DeepSeek model

## Dependencies

- `github.com/andybalholm/brotli` - Brotli compression support
- `github.com/joho/godotenv` - Environment variable management
- `golang.org/x/net` - HTTP/2 support

## Security

- The proxy includes CORS headers for cross-origin requests
- API keys are required and validated against environment variables
- Secure handling of request/response data
- Strict API key validation for all requests
- HTTPS support through HTTP/2
- Environment variables are never committed to the repository

## License

This project is licensed under the GNU General Public License v2.0 (GPLv2). See the [LICENSE.md](LICENSE.md) file for details.
