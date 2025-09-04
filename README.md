# Website Proxy Service

A transparent website proxy service built on Cloudflare Workers with Durable Objects. This service provides direct proxying to https://claude.ai/ with custom domain support and URL rewriting capabilities.

## ✨ Features

*   **Transparent Proxying**: All requests are directly proxied to https://claude.ai/
*   **URL Rewriting**: HTML content is modified so all links work through the proxy
*   **CORS Support**: Handles cross-origin requests properly
*   **Custom Domain Support**: Configured for c.hkcot.com and claude.hkcot.com domains
*   **No Authentication**: Direct access without any login or API keys required
*   **Durable Objects**: Uses Cloudflare Durable Objects for reliable request handling
*   **Enhanced URL Rewriting**: Handles complex web applications with JavaScript strings, JSON objects, and various HTML attributes

## 🚀 Deployment

You can deploy this project to your own Cloudflare account using the following methods:

### Method 1: Deploy via Wrangler CLI

1.  **Clone the project**
    ```bash
    git clone <repository-url>
    cd website-proxy
    ```

2.  **Install dependencies**
    ```bash
    pnpm install
    ```

3.  **Login to Wrangler**
    ```bash
    npx wrangler login
    ```

4.  **Deploy to Cloudflare**
    ```bash
    pnpm run deploy
    ```
    After successful deployment, Wrangler will output your Worker URL.

### Method 2: Deploy via Cloudflare Dashboard (Recommended)

1.  **Fork the project**: Click the "Fork" button in the top right corner of this repository to fork it to your own GitHub account.

2.  **Login to Cloudflare**: Open [Cloudflare Dashboard](https://dash.cloudflare.com/).

3.  **Create Worker**:
    *   In the left navigation bar, go to `Workers & Pages`.
    *   Click `Create Application` -> `Connect to Git`.
    *   Select your forked repository.
    *   In the "Build and Deploy" settings, Cloudflare will usually automatically detect this as a Worker project with no additional configuration needed.
    *   Click `Save and Deploy`.

## 🔧 Development

```bash
# Start development server
pnpm run dev

# Alternative development command
pnpm start

# Generate TypeScript types from Wrangler configuration
pnpm run cf-typegen
```

## 📖 How It Works

1. **Request Flow**: User visits your Worker URL → Request goes to WebsiteProxy Durable Object → Proxy fetches from claude.ai → Response sent back to user
2. **URL Rewriting**: HTML responses are processed to rewrite URLs so they point through your proxy
3. **Header Management**: Appropriate headers are set/removed to ensure proper proxying
4. **CORS Handling**: CORS headers are automatically added to all responses

## ⚙️ Configuration

The service is configured through `wrangler.jsonc`:

- **Durable Objects**: Uses `WebsiteProxy` Durable Object with SQLite migration
- **Custom Domains**: Configured for c.hkcot.com and claude.hkcot.com
- **Target URL**: Hardcoded to `https://claude.ai` in `src/handler.ts`
- **Observability**: Monitoring enabled for performance tracking

To change the target website, modify the `TARGET_BASE_URL` constant in `src/handler.ts`.

## 🏗️ Architecture

**Core Components:**
- `src/index.ts` - Simple routing that forwards all requests to Durable Object
- `src/handler.ts` - WebsiteProxy Durable Object that handles all proxy logic

**Technology Stack:**
- Cloudflare Workers + Durable Objects for serverless execution
- Hono web framework for basic routing
- TypeScript with strict configuration
- PNPM for package management

## 💻 Usage

Simply visit your deployed Worker URL and you'll see the exact same website as https://claude.ai/ - no setup or configuration needed. All links and resources will work seamlessly through the proxy.

**Note**: Claude.ai may have additional security measures or authentication requirements that could affect proxy functionality. The enhanced URL rewriting includes handling for JavaScript strings, JSON objects, and various HTML attributes to support complex web applications.
