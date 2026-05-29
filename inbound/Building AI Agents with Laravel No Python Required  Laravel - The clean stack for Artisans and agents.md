---
title: "Building AI Agents with Laravel: No Python Required | Laravel - The clean stack for Artisans and agents"
source: "https://laravel.com/blog/building-ai-agents-with-laravel-no-python-required"
author:
  - "[[Ana Tavares]]"
published: 2026-05-15
created: 2026-05-22
description: "The Laravel AI SDK is the LangChain PHP alternative: agents, tools, memory, streaming, and multi-agent workflows, entirely in PHP. No Python required."
tags:
  - "clippings"
---
![Building AI Agents with Laravel: No Python Required](https://fls-9f826fcc-b2ad-40d8-813f-9cf7dac049fa.laravel.cloud/posts/images/01KRP0NR21XYCRZHX7GPTCX4YS.png)

For the past couple of years, if you searched for how to build an AI agent, every tutorial, course, and conference talk pointed to Python. The tools that mattered, such as agent frameworks, vector store integrations, and multi-agent orchestration, were built for Python first, and sometimes only.

PHP developers who wanted to build something real were left with three options: learn Python, build a Python microservice and call it over HTTP from Laravel, or settle for raw API calls that barely scratched the surface of what agents can do.

The [Laravel AI SDK](https://laravel.com/ai) changes that. Released as a first-party package, it gives PHP developers a complete, idiomatic foundation for building AI agents: tools, memory, structured output, streaming, multi-agent workflows, vector search, and native integration with Laravel's queues, filesystem, and [Eloquent](https://laravel.com/docs/eloquent). You don’t need to use Python.

This guide covers what the Laravel AI SDK offers, how to build with it, and what you can realistically ship with it today.

### Why PHP Developers Felt Left Out

The framework PHP developers kept hearing about was LangChain, a Python library that introduced the vocabulary most developers now use for AI agents: chains, tools, memory, and vector stores. It was not available in PHP, and searching for a LangChain PHP equivalent turned up nothing production-ready. So every time a tutorial said, "here is how to build an agent," it implicitly said, "and you will need Python to do it."

The Laravel AI SDK solves this, covering the core use cases: agents with tools, conversation memory, structured output, and [multi-agent workflows](https://laravel.com/blog/building-multi-agent-workflows-with-the-laravel-ai-sdk) in PHP, with conventions that Laravel developers already know.

## What the Laravel AI SDK Is

The [Laravel AI SDK (`laravel/ai`](https://laravel.com/blog/introducing-the-laravel-ai-sdk)[) is a first-party package](https://laravel.com/docs/ai-sdk) that provides a unified PHP API for interacting with 14 AI providers: OpenAI, Anthropic, Google Gemini, Groq, Mistral, DeepSeek, xAI, Ollama, Azure OpenAI, Cohere, OpenRouter, Jina, VoyageAI, and ElevenLabs.

The SDK is a full agent framework built on Laravel's conventions, with a testing layer, conversation persistence, queue support, streaming, embeddings, vector stores, image generation, and audio transcription.

The core building block is an `Agent` class, a PHP class that encapsulates instructions, conversation history, tools, and an optional output schema. AI agents integrate natively with Laravel queues, filesystems, broadcasting, and Eloquent.

## Building Your First Laravel Agent

Since an agent in the Laravel AI SDK is a PHP class, you implement interfaces to declare what the agent can do, and the SDK handles the rest.

```
namespace App\Ai\Agents;

use Laravel\Ai\Contracts\Agent;
use Laravel\Ai\Contracts\HasTools;
use Laravel\Ai\Contracts\HasStructuredOutput;
use Laravel\Ai\Promptable;

class SupportAgent implements Agent, HasTools, HasStructuredOutput
{
    use Promptable;

    public function instructions(): string
    {
        return 'You are a support agent. Help users resolve billing issues.';
    }

    public function tools(): iterable
    {
        return [new LookupSubscription, new IssueRefund];
    }

    public function schema(JsonSchema $schema): array
    {
        return [
            'resolution'    => $schema->string()->required(),
            'ticket_closed' => $schema->boolean()->required(),
        ];
    }
}
```

To use it:

```
$response = SupportAgent::make(user: $user)->prompt($request->input('message'));

return $response['resolution'];
```

The agent class is defined once and called anywhere. Tools, instructions, and output schema live in the class (you’ll find examples in our [introduction to the AI SDK](https://laravel.com/blog/introducing-the-laravel-ai-sdk)). It resolves the agent through Laravel's service container, so dependencies are injected automatically.

## What Laravel Agents Can Do

**Tool calling:** Tools are PHP classes that implement a `handle` method and a JSON schema. The model decides when to call them. The SDK executes them and returns the result to the model automatically.

**Structured output:** Agents that implement `HasStructuredOutput` return typed, validated JSON. The `schema()` method defines the shape. Responses are accessible as array keys with the types you declared.

**Streaming:** Agents can stream responses directly to the browser as server-sent events. The SDK supports the Vercel AI data protocol out of the box, which means it works with Livewire and Inertia streaming setups without custom wiring.

```
return SupportAgent::make(user: $user)    ->stream($request->input('message'))    ->usingVercelDataProtocol();
```

**Conversation memory:** Add the `RemembersConversations` trait, and the Laravel AI SDK automatically stores and retrieves conversation history from your database.

```
// Start a conversation
$response = SupportAgent::make(user: $user)->prompt('Hello');$id = $response->conversationId;
// Continue it
$response = SupportAgent::make(user: $user)    ->continue($id)    ->prompt('What did I just say?');
```

**Queueing:** Long-running agents should not block HTTP requests. The AI SDK integrates natively with Laravel queues.

```
SupportAgent::make(user: $user)->queue($message)->then(fn ($response) => $user->notify(new AgentDone($response)));
```

**Provider failover:** Pass an array of providers, and the SDK tries them in order if one fails.

```
SupportAgent::make()->prompt($message,provider:[Lab::Anthropic, Lab::OpenAI]);
```

**Retrieval-augment generation (RAG) and vector search:** The `SimilaritySearch` tool plugs directly into Eloquent models backed by [pgvector](https://github.com/pgvector/pgvector). The `whereVectorSimilarTo` query scope handles the vector math.

```
Document::query()->whereVectorSimilarTo('embedding', 'billing cancellation policy', minSimilarity: 0.7)->limit(10)->get();
```

**Testing:** The AI SDK ships a complete fake layer with assertion helpers so you don’t need a paid observability platform for basic test coverage.

```
SupportAgent::fake(['Your subscription has been cancelled.']);SupportAgent::assertPrompted('cancel my subscription');
```

## Multi-Agent Workflows

Single agents handle most tasks. When you need more, the SDK ships five multi-agent patterns based on Anthropic's published agent design research. To learn more about this, read how you can [build multi-agent workflows with the SDK](https://laravel.com/blog/building-multi-agent-workflows-with-the-laravel-ai-sdk).

**Prompt chaining:** One agent's output feeds the next. Best for fixed sequences: draft, review, refine, format.

**Routing:** A classifier agent reads the input and routes it to the right specialist. Use `#[UseCheapestModel]` on the routing agent to keep costs low, since classification is a simple task.

**Parallelization:** Multiple agents run simultaneously using `Concurrency::run()`, PHP's equivalent of `Promise.all()`. Best for independent analyses of the same input -- for example, three agents reviewing code in parallel, with a fourth synthesizing the results.

**Orchestrator-workers:** A coordinator agent delegates to worker agents as tools. The model determines the execution path dynamically. No hardcoded workflows, no state machine graphs to maintain.

**Evaluator-optimizer:** A generate-evaluate-improve loop with a configurable iteration limit. The evaluator returns a structured score and approval flag. The loop continues until the content passes or hits the limit.

Taylor Otwell, creator of Laravel, noted why the framework’s conventions matter in an [SDK demo](https://www.youtube.com/watch?v=8MJ7r6niTus): "LLMs do well with things that are easy to parse and understand. Frameworks that lean into conventions and structure do well with LLMs."

## Getting Started

Install with `composer require laravel/ai` and follow the [official Laravel AI SDK documentation](https://laravel.com/docs/ai-sdk). The [multi-agent workflow guide](https://laravel.com/blog/building-multi-agent-workflows-with-the-laravel-ai-sdk) covers all five agent patterns with working code.

For developers who added a Python microservice specifically to handle AI features alongside their Laravel app, the Laravel AI SDK typically eliminates the need for that service. One language, one deployment, one codebase. Build one agent, test it with `SalesCoach::fake()`, and deploy it on the queue. You will have a working production agent in a single afternoon.

Need a live demo to help you put all together? Watch how you can [build an agentic loop with Laravel](https://youtu.be/J0W_Ety8j6Q) from scratch.

### Frequently Asked Questions

**What is the Laravel AI SDK?**

The Laravel AI SDK is the official first-party package for building AI-powered applications in Laravel. It provides agents, tool calling, structured output, streaming, vector search, image generation, audio transcription, and conversation memory, all within Laravel's conventions and integrated with its native subsystems.

**Is there a LangChain for PHP?**

Not officially. LangChain is a Python framework with no PHP port. The Laravel AI SDK is the closest equivalent for PHP developers: it covers the same core use cases, such as agents, tool calling, conversation memory, vector search, and multi-agent workflows, as a first-party, officially maintained Laravel package.

**Does the Laravel AI SDK support OpenAI and Anthropic?**

Yes. The SDK supports 14 providers, including OpenAI, Anthropic, Google Gemini, Groq, Mistral, DeepSeek, xAI, and Ollama. Switching providers is a single attribute or `.env` change. Automatic failover across providers is supported.

**Can I build AI agents in PHP without learning Python?**

Yes, for the vast majority of web application use cases. The Laravel AI SDK covers agents, tools, memory, multi-agent workflows, and vector search in PHP. The only cases that still require Python are direct integration with Python ML libraries (e.g., PyTorch, scikit-learn) or Python-specific tooling such as LangSmith.

**Can I use local models with the Laravel AI SDK?**

Yes. The SDK supports Ollama for local models and accepts any OpenAI-compatible endpoint via a base URL override in `config/ai.php`. This means you can run agents against local Llama, Mistral, or Phi models with no API costs.

**How does conversation memory work?**

Add the `RemembersConversations` trait to your agent class. The Laravel AI SDK automatically persists conversation history to your database and retrieves it on subsequent prompts. You manage conversations by ID, so multiple users can have independent sessions with the same agent class.

Ana Tavares

May 15, 2026

[Laravel AI](https://laravel.com/blog/category/laravel-ai)

8 min read

## Keep reading

[Laravel AI](https://laravel.com/blog/category/laravel-ai) May 14, 2026

### Introducing Laravel PAO: Cleaner Output for AI Agents

Laravel PAO formats PHPUnit, Pest, PHPStan, Rector, and Artisan output into structured JSON for AI agents automatically and without changing your terminal.

Nuno Maduro

[Laravel AI](https://laravel.com/blog/category/laravel-ai) May 8, 2026

### Laravel AI Integration: Build a Document Search Agent

Learn how to build a document search agent with the Laravel AI SDK. Upload files, generate vector embeddings, and stream answers to the browser in real time.

Laravel Team

[Laravel AI](https://laravel.com/blog/category/laravel-ai) Apr 30, 2026

### The Easiest and Fastest Way to Build MCP and Claude Apps

Laravel MCP Apps let your MCP tools return live, interactive HTML, all rendered inline inside Claude and VS Code Copilot. One command to scaffold.

Pushpak Chhajed