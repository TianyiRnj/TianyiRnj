# Tianyi Ren

**Software engineer — backend systems, AI infrastructure, and full-stack products.**

I design and ship production systems with clean service boundaries, durable data pipelines, and reliable APIs. My projects include **Musashi**, a prediction-market intelligence platform, and **Agent Run Cache**, a browser automation project built on HyperAgent.

---

## About

I hold a Master's in Electrical and Computer Engineering from Carnegie Mellon University and a B.S. in Computer Science and Mathematics from Penn State. I work across backend systems, AI infrastructure, full-stack product development, and applied ML, focusing on turning research-grade ideas into dependable, well-structured services.

---

## Featured Projects

### Musashi — Prediction-Market Intelligence Platform

[Homepage](https://musashi.bot/) · [GitHub](https://github.com/MusashiBot) · [Chrome Extension](https://chromewebstore.google.com/detail/musashi/jjnfhhfdfeglfncccjjpojbnngodaepk)

![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-5FA04E?style=flat-square&logo=nodedotjs&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3FCF8E?style=flat-square&logo=supabase&logoColor=white)
![MCP](https://img.shields.io/badge/MCP-D97757?style=flat-square)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black)
![Tailwind CSS](https://img.shields.io/badge/Tailwind%20CSS-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)
![Chrome Extension MV3](https://img.shields.io/badge/Chrome%20Extension%20MV3-4285F4?style=flat-square&logo=googlechrome&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=flat-square&logo=vercel&logoColor=white)
![Railway](https://img.shields.io/badge/Railway-0B0D0E?style=flat-square&logo=railway&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)

Musashi turns Kalshi/Polymarket market data and Twitter/X social context into structured, queryable prediction-market intelligence. I led major backend, API, infrastructure, and agent-tooling work with collaborators across a multi-repo system, organized around four core layers:

- **musashi-infra** — ingests and normalizes market data into durable storage
- **musashi-api** — shared business logic for market matching, sentiment, and wallet intelligence behind a consistent REST surface
- **musashi-extension** — Chrome extension for Twitter/X-native prediction-market analysis
- **musashi-mcp** — agent-facing tools that expose the platform's intelligence to AI clients

The design emphasizes clean service boundaries, durable data pipelines, deterministic signal generation, shared API contracts, caching, and reusable product surfaces.

### Agent Run Cache — Replayable Browser Automation

[GitHub](https://github.com/TianyiRnj/Agent_RunCache)

![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-5FA04E?style=flat-square&logo=nodedotjs&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat-square)
![CDP](https://img.shields.io/badge/CDP-4285F4?style=flat-square&logo=googlechrome&logoColor=white)
![XPath](https://img.shields.io/badge/XPath-555555?style=flat-square)
![Zod](https://img.shields.io/badge/Zod-3E67B1?style=flat-square&logo=zod&logoColor=white)
![MCP](https://img.shields.io/badge/MCP-D97757?style=flat-square)

![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=flat-square)
![Anthropic](https://img.shields.io/badge/Anthropic-D97757?style=flat-square)
![Gemini](https://img.shields.io/badge/Gemini-4285F4?style=flat-square)
![DeepSeek](https://img.shields.io/badge/DeepSeek-4D6BFE?style=flat-square)

A TypeScript browser automation project built on [HyperAgent](https://github.com/hyperbrowserai/HyperAgent), using Playwright and Chrome DevTools Protocol. Records LLM-driven workflows and replays cached element actions through XPath, with model fallback when cached execution fails. Supports replay-script generation for reusable automation workflows.

### moe-text-classifier — Mixture-of-Experts NLP Training

[github.com/TianyiRnj/moe-text-classifier](https://github.com/TianyiRnj/moe-text-classifier)

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)
![Hugging Face](https://img.shields.io/badge/Hugging%20Face-FFD21E?style=flat-square&logo=huggingface&logoColor=black)
![DeepSpeed](https://img.shields.io/badge/DeepSpeed-0E76A8?style=flat-square)
![Accelerate](https://img.shields.io/badge/Accelerate-FFD21E?style=flat-square&logo=huggingface&logoColor=black)
![CUDA](https://img.shields.io/badge/CUDA-76B900?style=flat-square&logo=nvidia&logoColor=white)

A Mixture-of-Experts text-classification project focused on scalable NLP training. Built with DeepSpeed, PyTorch, and transformer-based modeling, it explores efficient distributed training, mixed precision, checkpointing, selective layer freezing, and routing tradeoffs for large-scale text classification.

---

## More Projects

**Open-source repositories**

### [code-agent](https://github.com/TianyiRnj/code-agent)

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Claude](https://img.shields.io/badge/Claude-D97757?style=flat-square)

Autonomous coding agent in Python, powered by Claude; plans steps, edits code, runs validation, and self-corrects behind a policy guard.

### [dino-classification-extent](https://github.com/TianyiRnj/dino-classification-extent)

![DINO](https://img.shields.io/badge/DINO-8B5CF6?style=flat-square)
![Computer Vision](https://img.shields.io/badge/Computer%20Vision-2563EB?style=flat-square)

Self-supervised vision using DINO-style image classification.

### [RF_heart_sensing](https://github.com/TianyiRnj/RF_heart_sensing)

![C](https://img.shields.io/badge/C-A8B9CC?style=flat-square&logo=c&logoColor=black)
![RF Sensing](https://img.shields.io/badge/RF%20Sensing-2563EB?style=flat-square)

RF-based heart sensing implemented in C.

### [Custom-Chrome-UI](https://github.com/TianyiRnj/Custom-Chrome-UI)

![Chrome Extensions](https://img.shields.io/badge/Chrome%20Extensions-4285F4?style=flat-square&logo=googlechrome&logoColor=white)

Custom browser UI built as a Chrome extension.

**Selected engineering work**

### Authentication & trusted-device system

![Java](https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Vue.js](https://img.shields.io/badge/Vue.js-4FC08D?style=flat-square&logo=vuedotjs&logoColor=white)
![REST APIs](https://img.shields.io/badge/REST%20APIs-009688?style=flat-square)

Spring Boot + Vue.js MFA with OTP verification, trusted-device management, secure REST APIs, and audit logging.

### AI-assisted scheduling & collaboration platform

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)

Full-stack FastAPI/PostgreSQL system with AI-assisted scheduling and real-time collaboration.

**Additional tools:** Flask · Next.js · Docker

---

## Contact

**Connect:** [Email](mailto:rentynj@gmail.com) · [LinkedIn](https://www.linkedin.com/in/rentianyi/) · [GitHub](https://github.com/TianyiRnj)

**Featured product:** [Musashi](https://musashi.bot/) · [Musashi GitHub](https://github.com/MusashiBot) · [Chrome Extension](https://chromewebstore.google.com/detail/musashi/jjnfhhfdfeglfncccjjpojbnngodaepk)
