# 🛡 Agent Resilience Kit (ARK)

**Break your AI agent on purpose. Ship one that survives.**

Agents get tested on the happy path and shipped — then they fabricate results and obey hidden instructions the moment something degrades. ARK is five open-source tools that throw the storm at your own agent, in a system you own, so you find the failures before your users do.

Each tool speaks the OpenAI chat API, so it drops in front of Ollama, vLLM, OpenAI, or DeepSeek with no code changes. All MIT.

## The kit

| | tool | role | what it does |
|---|---|---|---|
| 📼 | [**agent-blackbox**](https://github.com/KhaiB10/agent-blackbox) | record | a flight recorder — captures every agent turn (messages, tools, replies, timing); keys never hit disk |
| 🌪 | [**tornado**](https://github.com/KhaiB10/tornado) | break | chaos engineering — injects tools-vanishing, prompt truncation, tool-calls-as-text, latency, errors. Ships as a GitHub Action |
| 🦈 | [**shark**](https://github.com/KhaiB10/shark) | watch | scans recorded traffic for leaked secrets (redacted), obeyed injections, claimed-but-never-done actions, unoffered tools |
| 🐍 | [**viper**](https://github.com/KhaiB10/viper) | strike | fires 10 prompt-injection techniques at your endpoint; each plants a harmless marker, so you see which land |
| 🌀 | [**hurricane**](https://github.com/KhaiB10/hurricane) | grade | one resilience grade, A–F, from injection resistance + honesty (does it fabricate when it can't do the task?) |
| 🌊 | [**tsunami**](https://github.com/KhaiB10/tsunami) | surge | floods the endpoint under concurrency and catches silent prompt truncation on oversized input |

Plus [**ollama-guard**](https://github.com/KhaiB10/ollama-guard) — the fix for the truncation bug that started all of this: it makes `num_ctx` work on Ollama's `/v1`, recovers tool calls the model wrote as text, and flags silent truncation. `npx ollama-guard doctor` tells you in 5 seconds if you're affected.

## The pipeline

```
📼 record  →  🌪 break  →  🦈 watch  →  🐍 strike  →  🌀 grade (gate the PR)
```

## Quick start

```bash
# check if a truncated prompt is already making your agent fabricate
npx ollama-guard doctor

# one resilience grade, A–F
npm i -g @khaib10/hurricane
hurricane --model qwen3:8b --guard-file your-system-prompt.txt

# the rest of the kit
npm i -g @khaib10/tornado @khaib10/shark @khaib10/viper @khaib10/hurricane @khaib10/tsunami @khaib10/agent-blackbox

# gate every pull request on resilience
# .github/workflows/agent-resilience.yml
#   - uses: KhaiB10/tornado@v1
```

## Backed by pre-registered research

The failures ARK hunts were measured first, in [tool-call-truncation-bench](https://github.com/KhaiB10/tool-call-truncation-bench) — a 936-trial benchmark whose plan and scoring were hashed and timestamped in Bitcoin block [969084](https://mempool.space/block/969084) **before** any data was collected, so the goalposts couldn't move. One finding: asked to hash a file with no tool, a local model returned `8f9c6004636b`; the real hash was `74b1d9a141ab`. It invented one and reported it as fact.

## Honesty

Every tool runs against systems **you own or are authorized to test.** ARK creates controlled failures in your own agent to harden it — the same idea as chaos engineering for servers. It is not for attacking anyone else's.

Built by Khai Bustos. MIT licensed.
