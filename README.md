<div align="center">
  <h1>Agent Run Cache</h1>

  <p align="center">
    <strong>Intelligent Browser Automation with LLMs</strong>
  </p>
</div>

## Overview

Agent Run Cache is Playwright supercharged with AI. No more brittle scripts, just powerful natural language commands.

Agent Run Cache is distributed as source code from this GitHub repository and runs on your own machine. See [License and Attribution](#license-and-attribution) for its upstream origin.

### Features

- 🤖 **AI Commands**: Simple APIs like `page.ai()`, `page.extract()` and `executeTask()` for any AI automation
- ⚡ **Fallback to Regular Playwright**: Use regular Playwright when AI isn't needed
- 🥷 **Stealth Mode** – Avoid detection with built-in anti-bot patches
- ☁️ **Cloud Ready** – Optionally run sessions on remote browsers through the Hyperbrowser provider
- 🔌 **MCP Client** – Connect to tools like Composio for full workflows (e.g. writing web data to Google Sheets)
- 📼 **Action Caching** – Record and replay workflows deterministically without LLM calls

## Quick Start

### Install from Source

Download or clone this repository, install its dependencies, build it, configure
your environment, and run it locally:

```bash
# Download the source.
git clone https://github.com/TianyiRnj/Agent_RunCache.git
cd Agent_RunCache

# Install dependencies and build.
yarn install --frozen-lockfile
yarn build

# Create the local environment file once.
cp .env.example .env

# Open .env and set OPENAI_API_KEY and OPENAI_MODEL. The CLI loads them automatically.

# Run Agent Run Cache from the local source.
yarn cli -c "Open example.com and return the page title"
```

The minimum `.env` configuration for the default CLI is:

```dotenv
OPENAI_MODEL=gpt-4o
OPENAI_API_KEY=your-api-key
```

The repository ignores `.env`, so local secrets are not committed. Keep
`.env.example` as the shareable list of supported variables. After configuring
`.env` once, subsequent commands do not require `export OPENAI_API_KEY=...`.
`OPENAI_MODEL` controls the model used by the CLI and by `new AgentRunCache()` when
no explicit `llm` configuration is supplied; it defaults to `gpt-4o` when unset.

The code samples in this README, the files in [`examples/`](examples/), and
generated replay scripts run under this repository's development configuration.
`agent-run-cache` is the local package name and resolves to `src/index.ts`
through the repository's TypeScript path configuration, so these files are not
expected to work outside a checkout of this repository:

```bash
yarn example examples/simple/add-to-amazon-cart.ts

# Test a login flow using SauceDemo's public demo credentials from .env.
yarn example examples/simple/login-to-saucedemo.ts
```

#### Run in an isolated Node environment

Agent Run Cache is a Node.js/TypeScript project, so a Python `venv` does not isolate
its runtime or dependencies. Use a Node version manager such as `nvm`; project
dependencies will remain isolated in this repository's `node_modules` directory.

```bash
# Create/select an isolated Node runtime for this shell.
nvm install 22
nvm use 22

# This repository uses a Yarn v1 lockfile.
corepack enable
corepack prepare yarn@1.22.22 --activate

cd Agent_RunCache
yarn install --frozen-lockfile
yarn build

# Reuse the .env file created during local setup.
yarn cli -c "Open example.com and return the page title"
```

If all dependency archives are already present in the local Yarn cache, the
dependency installation can be performed without network access:

```bash
yarn install --offline --frozen-lockfile
```

### CLI

```bash
yarn cli -c "Find a route from Miami to New Orleans, and provide the detailed route information."
```

The CLI supports options for debugging or using hyperbrowser instead of a local browser

```bash
-d, --debug                       Enable debug mode
-c, --command <task description>  Command to run
--hyperbrowser                    Use Hyperbrowser for the browser provider
```

### Library

```typescript
import { AgentRunCache } from "agent-run-cache";
import { z } from "zod";

// Initialize the agent
const agent = new AgentRunCache({
  llm: {
    provider: "openai",
    model: "gpt-4o",
  },
});

// Execute a task
const result = await agent.executeTask(
  "Navigate to amazon.com, search for 'laptop', and extract the prices of the first 5 results"
);
console.log(result.output);

// Use page.ai, page.perform, and page.extract
const page = await agent.newPage();
await page.goto("https://flights.google.com", { waitUntil: "load" });
await page.ai("search for flights from Rio to LAX from July 16 to July 22");
await page.perform("click the search button");
const res = await page.extract(
  "give me the flight options",
  z.object({
    flights: z.array(
      z.object({
        price: z.number(),
        departure: z.string(),
        arrival: z.string(),
      })
    ),
  })
);
console.log(res);

// Clean up
await agent.closeAgent();
```

## Two Modes of Operation

Agent Run Cache provides two complementary APIs optimized for different use cases:

### 🎯 `page.perform()` - Single Granular Actions

> `page.aiAction()` is deprecated and remains available as an alias; prefer `page.perform()` going forward.

**Best for**: Single, specific actions like "click login", "fill email with test@example.com"

**Advantages**:
- ⚡ **Fast** - Uses accessibility tree (no screenshots)
- 💰 **Cheap** - Single LLM call per action
- 🎯 **Reliable** - Direct element finding and execution
- 📊 **Efficient** - Text-based DOM analysis with automatic ad-frame filtering

**Example**:
```typescript
const page = await agent.newPage();
await page.goto("https://example.com/login");

// Fast, reliable single actions
await page.perform("fill email with user@example.com");
await page.perform("fill password with mypassword");
await page.perform("click the login button");
```

### 🧠 `page.ai()` - Complex Multi-Step Tasks

**Best for**: Complex workflows requiring multiple steps and visual context

**Advantages**:

- 🖼️ **Visual Understanding** - Can use screenshots with element overlays
- 🎭 **Complex Tasks** - Handles multi-step workflows automatically
- 🧠 **Context-Aware** - Better at understanding page layout and relationships
- 🔄 **Adaptive** - Can adjust strategy based on page state

**Parameters**:

- `useDomCache` (boolean): Reuse DOM snapshots for speed
- `enableVisualMode` (boolean): Enable screenshots and overlays (default: false)

**Example**:

```typescript
const page = await agent.newPage();
await page.goto("https://flights.google.com");

// Complex task with multiple steps handled automatically
await page.ai("search for flights from Miami to New Orleans on July 16", {
  useDomCache: true,
});
```

### 🎨 Mix and Match

Combine both APIs for optimal performance:

```typescript
// Use perform for fast, reliable individual actions
await page.perform("click the search button");
await page.perform("type laptop into search");

// Use ai() for complex, multi-step workflows
await page.ai("filter results by price under $1000 and sort by rating");

// Extract structured data
const products = await page.extract(
  "get the top 5 products",
  z.object({
    products: z.array(z.object({ name: z.string(), price: z.number() }))
  })
);
```

## ☁️ Cloud

You can scale Agent Run Cache with cloud headless browsers using Hyperbrowser

1. Get a free api key from [Hyperbrowser](https://app.hyperbrowser.ai/)
2. Add it to your env as `HYPERBROWSER_API_KEY`
3. Set your `browserProvider` to `"Hyperbrowser"`

```typescript
const agent = new AgentRunCache({
  browserProvider: "Hyperbrowser",
});

const response = await agent.executeTask(
  "Go to hackernews, and list me the 5 most recent article titles"
);

console.log(response);
await agent.closeAgent();
```

## Usage Guide

### Multi-Page Management

```typescript
// Create and manage multiple pages
const page1 = await agent.newPage();
const page2 = await agent.newPage();

// Execute tasks on specific pages
const page1Response = await page1.ai(
  "Go to google.com/travel/explore and set the starting location to New York. Then, return to me the first recommended destination that shows up. Return to me only the name of the location."
);
const page2Response = await page2.ai(
  `I want to plan a trip to ${page1Response.output}. Recommend me places to visit there.`
);

console.log(page2Response.output);

// Get all active pages
const pages = await agent.getPages();
await agent.closeAgent();
```

## Customization

### Output Schema Definition

Agent Run Cache can extract data in a specified schema. The schema can be passed in at a per-task level

```typescript
import { z } from "zod";

const agent = new AgentRunCache();
const agentResponse = await agent.executeTask(
  "Navigate to imdb.com, search for 'The Matrix', and extract the director, release year, and rating",
  {
    outputSchema: z.object({
      director: z.string().describe("The name of the movie director"),
      releaseYear: z.number().describe("The year the movie was released"),
      rating: z.string().describe("The IMDb rating of the movie"),
    }),
  }
);
console.log(agentResponse.output);
await agent.closeAgent();
```

```bash
{
  "director": "Lana Wachowski, Lilly Wachowski",
  "releaseYear": 1999,
  "rating": "8.7/10"
}
```

### Using Different LLM Providers

Agent Run Cache supports multiple LLM providers with native SDKs for better performance and reliability.

```typescript
// Using OpenAI
const agent = new AgentRunCache({
  llm: {
    provider: "openai",
    model: "gpt-4o",
  },
});

// Using Anthropic's Claude
const agent = new AgentRunCache({
  llm: {
    provider: "anthropic",
    model: "claude-sonnet-4-0",
  },
});

// Using Google Gemini
const agent = new AgentRunCache({
  llm: {
    provider: "gemini",
    model: "gemini-2.5-flash",
  },
});

// Using DeepSeek
const agent = new AgentRunCache({
  llm: {
    provider: "deepseek",
    model: "deepseek-chat",
  },
});
```

### MCP Support

Agent Run Cache functions as a fully functional MCP client. For best results, we recommend using
`gpt-4o` as your LLM.

Here is an example which reads from wikipedia, and inserts information into a google sheet using the composio Google Sheet MCP. For the full example, see [here](examples/mcp/google-sheets/most-populated-states.ts)

```typescript
const agent = new AgentRunCache({
  llm: llm,
  debug: true,
});

await agent.initializeMCPClient({
  servers: [
    {
      command: "npx",
      args: [
        "@composio/mcp@latest",
        "start",
        "--url",
        "https://mcp.composio.dev/googlesheets/...",
      ],
      env: {
        npm_config_yes: "true",
      },
    },
  ],
});

const response = await agent.executeTask(
  "Go to https://en.wikipedia.org/wiki/List_of_U.S._states_and_territories_by_population and get the data on the top 5 most populous states from the table. Then insert that data into a google sheet. You may need to first check if there is an active connection to google sheet, and if there isn't connect to it and present me with the link to sign in. "
);

console.log(response);
await agent.closeAgent();
```

### Custom Actions

Agent Run Cache's capabilities can be extended with custom actions. Custom actions require 3 things:

- type: Name of the action. Should be something descriptive about the action.
- actionParams: A zod object describing the parameters that the action may consume.
- run: A function that takes in a context, and the params for the action and produces a result based on the params.

Here is an example that performs a search using Exa

```typescript
const exaInstance = new Exa(process.env.EXA_API_KEY);

export const RunSearchActionDefinition: AgentActionDefinition = {
  type: "perform_search",
  actionParams: z.object({
    search: z
      .string()
      .describe(
        "The search query for something you want to search about. Keep the search query concise and to-the-point."
      ),
  }).describe("Search and return the results for a given query."),
  run: async function (
    ctx: ActionContext,
    params: { search: string }
  ): Promise<ActionOutput> {
    const results = (await exaInstance.search(params.search, {})).results
      .map(
        (res) =>
          `title: ${res.title} || url: ${res.url} || relevance: ${res.score}`
      )
      .join("\n");

    return {
      success: true,
      message: `Successfully performed search for query ${params.search}. Got results: \n${results}`,
    };
  },
};

const agent = new AgentRunCache({
  customActions: [RunSearchActionDefinition],
});

const result = await agent.executeTask(
  "Search about the news for today in New York"
);
```

## 📼 Action Caching

Agent Run Cache automatically records every action during `page.ai()` runs, capturing XPaths, frame indices, and execution details. This enables deterministic replay without LLM calls—perfect for regression testing, CI pipelines, and cost optimization.

### How It Works

Every `page.ai()` run produces an `actionCache` containing the exact sequence of actions performed:

```typescript
const page = await agent.newPage();
const { actionCache } = await page.ai(
  "Go to flights.google.com and search for flights from Rio to LAX"
);

// actionCache contains the recorded steps
console.log(actionCache);
```

Example cache entry:
```json
{
  "taskId": "86d13abe-b9f3-4ca3-a9bb-bdeddf234cd1",
  "createdAt": "2025-12-06T05:44:52.257Z",
  "status": "completed",
  "steps": [
    {
      "stepIndex": 0,
      "instruction": "Click on the departure field",
      "elementId": "0-138",
      "method": "click",
      "arguments": [],
      "frameIndex": 0,
      "xpath": "/html[1]/body[1]/div[1]/input[1]",
      "actionType": "actElement",
      "success": true
    }
  ]
}
```

### Replaying Cached Actions

Replay a recorded session using `runFromActionCache()`. It attempts XPath-based execution first (no LLM calls), falling back to LLM only if the page structure has changed:

```typescript
import { AgentRunCache } from "agent-run-cache";
import { ActionCacheOutput } from "agent-run-cache/types";
import fs from "fs";

// Load a previously saved action cache
const cache: ActionCacheOutput = JSON.parse(
  fs.readFileSync("action-cache.json", "utf-8")
);

const agent = new AgentRunCache();
const page = await agent.newPage();

// Replay the cached actions
const replay = await page.runFromActionCache(cache, {
  maxXPathRetries: 3,  // Retry XPath resolution up to 3 times before LLM fallback
  debug: true,
});

console.log(replay);
// {
//   replayId: "...",
//   sourceTaskId: "86d13abe-...",
//   status: "completed",
//   steps: [{ stepIndex: 0, usedXPath: true, fallbackUsed: false, success: true }]
// }

await agent.closeAgent();
```

### Generating Replay Scripts

Generate a TypeScript script from an action cache for easy integration into your test suite:

```typescript
const { actionCache } = await page.ai("search for flights from Miami to NYC");

// Generate a replay script
const script = agent.createScriptFromActionCache(actionCache.steps);
console.log(script);
```

This produces a script using typed helper methods like `performClick()`, `performType()`, etc. The script imports from `agent-run-cache`, so run it from inside this repository with `yarn example <path>`.

### Use Cases

- **Regression Testing**: Record a workflow once, replay it in CI without LLM costs
- **Flaky Test Debugging**: Compare XPath-based replay vs LLM-driven execution
- **Cost Optimization**: Cache expensive multi-step workflows and replay deterministically
- **Workflow Templates**: Save common flows (login, checkout) and replay across environments

## CDP First

Agent Run Cache speaks Chrome DevTools Protocol natively. Element lookup, scrolling, typing, frame management, and screenshots all go through CDP so every action has exact coordinates, execution contexts, and browser events. This allows for more custom commands and deep iframe tracking.

Agent Run Cache integrates seamlessly with Playwright, so you can still use familiar commands, while the actions take full advantage of native CDP protocol with fast locators and advanced iframe tracking.

**Key Features:**

- **Auto-Ad Filtering**: Automatically filters out ad and tracking iframes to keep context clean
- **Deep Iframe Support**: Tracking across nested and cross-origin iframes (OOPIFs)
- **Exact Coordinates**: Actions use precise CDP coordinates for reliability

Keep in mind that CDP is still experimental, and stability is not guaranteed. If you'd like the agent to use Playwright's native locators/actions instead, set `cdpActions: false` when you create the agent and it will fall back automatically.

The CDP layer is still evolving—expect rapid polish (and the occasional sharp edge). If you hit something quirky you can toggle CDP off for that workflow and drop us a bug report.

## Contributing

We welcome contributions to Agent Run Cache! Here's how you can help:

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## Support

- 📚 [CDP Architecture Notes](docs/cdp-overview.md)
- 🧪 [Examples](examples/)
- 🐛 [Issue Tracker](https://github.com/TianyiRnj/Agent_RunCache/issues)

## License and Attribution

Agent Run Cache is a modified version of [HyperAgent](https://github.com/hyperbrowserai/HyperAgent), Copyright (c) 2025 S2 Labs Inc., and is distributed under the GNU Affero General Public License, version 3. See [LICENSE](LICENSE) for the upstream notice, a description of the modifications, and the full license text.
