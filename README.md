# Skill Flow · Declarative AI Task Orchestration

[![PyPI version](https://img.shields.io/pypi/v/skill-flow)](https://pypi.org/project/skill-flow/)
[![Python versions](https://img.shields.io/pypi/pyversions/skill-flow)](https://pypi.org/project/skill-flow/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub stars](https://img.shields.io/github/stars/yourname/skill-flow)](https://github.com/yourname/skill-flow/stargazers)

**Describe your intent in YAML, let AI orchestrate the rest.**  
Break free from node‑spaghetti of low‑code platforms. Return to clean, text‑based abstraction.

---

## 📖 Introduction

Skill Flow is a developer‑first, declarative framework for building complex AI workflows. You define *what* you want to accomplish in a simple YAML file – e.g. “plan a multi‑city trip” – and the underlying engine (LLM + MCP tools) figures out *how* to execute it, step by step.

**Core value:**
- 🔥 **Declarative** – focus on business logic, not implementation details.
- 🧩 **Composable** – Skills nest like Lego bricks.
- 🤖 **AI‑native** – the engine uses an LLM to interpret your DSL and dynamically selects tools.
- 📦 **Lightweight** – pure Python, easy to integrate and extend.

---

## 🚀 Quick Example

Create a file `travel_planner.yaml`:

```yaml
name: Multi‑city trip planner
steps:
  - skill: flight_search
    params:
      from: ${user.departure}
      to: ${user.destination}
    output: flights

  - skill: hotel_search
    params:
      location: ${flights[0].destination}
      dates: ${user.dates}
    output: hotels

  - skill: weather_forecast
    params:
      city: ${flights[0].destination}
      date: ${user.dates[0]}
    output: weather

  - respond: |
      Your flights: ${flights}
      Recommended hotels: ${hotels}
      Weather at destination: ${weather}
```

Then run it with Python:

```python
from skill_flow import Engine

engine = Engine(llm=my_llm, mcp_client=my_mcp_client)   # supports any LangChain model
result = await engine.run("travel_planner.yaml", user_input="May holiday from Beijing to Shanghai")
print(result)
```

Example output:
```
Your flights: CA1234 (08:00 Beijing → 10:30 Shanghai, on time)
Recommended hotels: W Hotel Bund (¥1200/night), Jinjiang Hotel (¥800/night)
Weather at destination: May 1st, cloudy 18‑24°C
```

---

## ❓ Problems It Solves

| Pain point | How Skill Flow helps |
|------------|----------------------|
| **Node explosion** in low‑code tools | Text‑based DSL avoids visual clutter; complexity scales linearly. |
| **Mixed abstraction levels** | Only Skill interfaces are exposed; atomic operations stay hidden. |
| **Imperative thinking** | Declare intent, let the LLM plan the execution dynamically. |
| **Poor composability** | Skills can be nested and reused like functions. |
| **Underutilised AI** | The engine feeds DSL fragments to the LLM, leveraging its understanding. |

---

## ✨ Core Philosophy

- **Declarative > Imperative** – tell the computer *what* you want, not *how* to do it.
- **Intent‑level abstraction** – focus on business logic, everything else is handled by LLM + MCP.
- **Composability first** – Skills are atomic units that can be combined arbitrarily.
- **Text is code** – all definitions are plain text, enabling version control, code review, and automation.

---

## 🧱 Design Principles

1. **Skill as atomic unit** – each Skill wraps an MCP tool or fixed logic with a clear input/output schema.
2. **DSL stays at business level** – YAML contains only Skill calls and data flow, no implementation details.
3. **LLM in the loop** – the engine does not hard‑code execution order; it asks the LLM to interpret each step.
4. **Progressive complexity** – start with linear flows, later add conditions, loops, parallelism, and sub‑workflows.
5. **Observability built‑in** – every step logs duration, token usage, and intermediate results for easy debugging.

---

## 📋 Roadmap (TODO)

### Phase 1: Core Engine (MVP)
- [ ] Define Skill metadata format (name, description, input/output schema, linked MCP tool)
- [ ] Implement YAML parser → internal AST
- [ ] Design engine‑LLM interaction protocol (how to turn a DSL fragment into a prompt)
- [ ] Support linear execution (Skills called one after another)
- [ ] Implement variable substitution (`${}` expressions)
- [ ] Integrate `mcp-use` as the underlying MCP client
- [ ] Emit final result via `respond` step

### Phase 2: Tooling & Skill Management
- [ ] Skill registry (load local or remote Skill definitions dynamically)
- [ ] Install third‑party Skills from GitHub / npm
- [ ] Skill versioning
- [ ] Skill sandbox for testing (simulate execution, validate I/O)
- [ ] CLI: `skill-flow run <file>`, `skill-flow list-skills`

### Phase 3: Advanced Control Flow
- [ ] Conditional branching (`if`/`else`)
- [ ] Loops (`for`/`while` over lists or conditions)
- [ ] Parallel execution (`parallel` steps)
- [ ] Sub‑workflows (call another YAML file as a step)
- [ ] Error handling & retries (`retry`, `on-error`)

### Phase 4: Production Features
- [ ] State management (share data across steps, support multi‑turn conversations)
- [ ] Caching (skip steps with identical inputs)
- [ ] Streaming intermediate results
- [ ] Visual debugger (real‑time DAG inspection)
- [ ] Deep MCP integration (auto‑discover tools from MCP servers)

### Phase 5: Ecosystem & Tooling
- [ ] VS Code extension (syntax highlighting, autocomplete, debugging)
- [ ] GitHub Actions integration (automated Skill testing)
- [ ] Online Playground (try Skill Flow in the browser)
- [ ] Documentation site + example gallery

---

## 🎥 Live Coding Sessions

I’m building Skill Flow in public! Watch the journey:

- Architecture discussions
- Live coding & debugging
- Tackling unexpected challenges
- Community Q&A

**Platforms**: [YouTube](https://youtube.com/@yourchannel) / [Bilibili](https://space.bilibili.com/yourid)  
**Schedule**: Every Tuesday & Thursday, 8:00 PM (Beijing time)  
**Recordings**: Available on the [GitHub Wiki](https://github.com/yourname/skill-flow/wiki)

Star the repo to get notified of new episodes!

---

## 🛠️ Installation & Usage

### From source
```bash
git clone https://github.com/yourname/skill-flow.git
cd skill-flow
pip install -e .
```

### From PyPI (once published)
```bash
pip install skill-flow
```

### Run an example
```bash
skill-flow run examples/travel_planner.yaml --user-input "May holiday from Beijing to Shanghai"
```

---

## 🤝 Contributing

We welcome all contributions! Please read [CONTRIBUTING.md](CONTRIBUTING.md) and our [Code of Conduct](CODE_OF_CONDUCT.md).

- 🐛 **Report bugs** – use the issue template.
- 💡 **Suggest features** – describe the use case.
- 🔧 **Submit PRs** – code, docs, examples are all appreciated.
- 💬 **Join discussions** – share your ideas in [Discussions](https://github.com/yourname/skill-flow/discussions).

---

## 📄 License

[MIT License](LICENSE) © 2025 [Your Name/Organization]

---

## 🌟 Acknowledgements

- [mcp-use](https://github.com/yourname/mcp-use) – lightweight Python MCP client
- [LangChain](https://github.com/langchain-ai/langchain) – LLM abstraction
- All contributors and early adopters ❤️

---

> **Skill Flow** isn’t just another framework – it’s a rethinking of how we build AI applications.  
> Let’s leave the complexity to code, and bring simplicity back to developers.
