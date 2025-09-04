# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a transparent website proxy service built on Cloudflare Workers with Durable Objects. Currently configured to proxy https://claude.ai/ with custom domain support. The service provides direct proxying with URL rewriting and CORS handling.

## Development Commands

```bash
# Development server
pnpm run dev          # Start local development server with Wrangler
pnpm start           # Alternative to pnpm run dev

# Deployment  
pnpm run deploy      # Deploy to Cloudflare Workers

# Type generation
pnpm run cf-typegen  # Generate TypeScript types from Wrangler configuration
```

## Architecture

**Core Components:**
- `src/index.ts` - Simple routing that forwards all requests to Durable Object
- `src/handler.ts` - WebsiteProxy Durable Object that handles all proxy logic

**Technology Stack:**
- **Cloudflare Workers** + **Durable Objects** for serverless execution
- **Hono** web framework for basic routing  
- **TypeScript** with strict configuration
- **PNPM** for package management

**Key Features:**
- **Transparent Proxying**: All requests are directly proxied to https://claude.ai/
- **URL Rewriting**: HTML content is modified so all links work through the proxy
- **CORS Support**: Handles cross-origin requests properly
- **Custom Domain Support**: Configured for c.hkcot.com and claude.hkcot.com domains
- **No Authentication**: Direct access without any login or API keys required

## How It Works

1. **Request Flow**: User visits your Worker URL → Request goes to WebsiteProxy Durable Object → Proxy fetches from claude.ai → Response sent back to user
2. **URL Rewriting**: HTML responses are processed to rewrite URLs so they point through your proxy
3. **Header Management**: Appropriate headers are set/removed to ensure proper proxying
4. **CORS Handling**: CORS headers are automatically added to all responses

## Configuration

**Wrangler Configuration (wrangler.jsonc):**
- Uses `WebsiteProxy` Durable Object with SQLite migration
- Custom domains: c.hkcot.com and claude.hkcot.com
- Target URL hardcoded to `https://claude.ai` in handler.ts
- Observability enabled for monitoring

**Key Configuration Points:**
- Target URL defined as `TARGET_BASE_URL` constant in `src/handler.ts`
- Durable Object namespace binding: `WEBSITE_PROXY`
- Custom domain routes configured in wrangler.jsonc

## Code Style

- **TypeScript** strict mode with ES2021 target
- **Minimal Dependencies**: Only Hono framework for routing
- **Clean Architecture**: Simple request forwarding with URL rewriting