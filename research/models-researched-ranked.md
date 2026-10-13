# AI models - what each one is good at, researched and ranked

Researched 2026-10-13 by checking each maker's own pages and 2026 benchmark writeups.

## Read this first

- Your OmniRoute gateway lists **43 model names**, but they are really **21 model families**.
- Names ending in `-high`, `-xhigh` or `-max` are the **same model** run at a higher
  "thinking effort" (it reasons more, so it is slower and costs more). The plain name is
  the default effort. Example: `claude-opus-5`, `claude-opus-5-high` and `claude-opus-5-max`
  are all Claude Opus 5.
- "Rank" below means **best to worst for that one skill**. The top few are close and can
  swap places depending on the exact task. These are cloud models; on this phone you reach
  them through OmniRoute, not by running them.

---

## Part 1 - The 21 families behind the 43 names

| Family (original names) | Maker | Out | Context | Price in/out per 1M | Best at |
|---|---|---|---|---|---|
| claude-fable-5, -high, claude-fable-5.1-max | Anthropic | Apr 2026 / 5.1 Aug 30 2026 | 1M | $10 / $50 | Anthropic's top GA model; hardest coding + long autonomous runs |
| claude-opus-4-6, -high | Anthropic | Feb 5 2026 | 1M | $5 / $25 | Early-2026 flagship; agentic coding + deep reasoning |
| claude-opus-4-7, -high | Anthropic | Apr 16 2026 | 1M | $5 / $25 | Added xhigh effort and much better vision |
| claude-opus-5, -high, -max | Anthropic | Jul 24 2026 | 1M | $5 / $25 | Near-Fable intelligence at half the price; SOTA coding |
| claude-opus-5.5, -high, -max | Anthropic | 2026 | 1M | $4 / $20 | Newest Opus; leads agentic coding, computer use, prompt-injection defence |
| claude-sonnet-5.5, -high, -xhigh | Anthropic | Sep 28 2026 | 1M | $2 / $10 | Best speed-for-intelligence; 30% faster, cheaper |
| gpt-5.6-sol, -xhigh | OpenAI | Jul 9 2026 | 1.05M | $4 / $20 | GPT-5.6 flagship; set the coding-agent record at launch |
| gpt-6-astra, -max | OpenAI | Sep 3 2026 | 1.05M | $10 / $50 | OpenAI's most intelligent model; best computer use |
| gpt-6-sol, -max | OpenAI | Sep 22 2026 | 1.05M | $2 / $10 | Cost-efficient high end, just below Astra |
| gpt-6.1-sol, -max | OpenAI | Sep 29 2026 | 1.05M | $2 / $10 | Near-Astra agentic work at one-fifth the price |
| gemini-3-pro | Google | Nov 18 2025 | 1M | $1 / $6 | Gemini 3 foundation; now legacy/cheap |
| gemini-3.1-pro-preview | Google | Feb 19 2026 | 1M | $2 / $12 | Frontier reasoning and agentic coding |
| gemini-3.7-flash, -high | Google | Aug 13 2026 | 1M | $0.75 / $3.75 | Everyday coding + agents, low cost |
| gemini-3.8-flash, -high | Google | Sep 9 2026 | 1M | $0.75 / $3.75 | Google's best Flash; beats Opus 5 on some tests |
| gemini-4-argon-high | Google | 2026 | 1M | (gated) | Frontier long-horizon reasoning, coding, science, cyber |
| grok-4.7, -xhigh | xAI | Sep 21 2026 | 500K | $2 / $6 | Coding + agents, best at checking its own work |
| kimi-k3, -max | Moonshot | Jul 16 2026 | 1M | open weights | Biggest open model (2.8T); agentic coding |
| hy4-preview | Tencent | Aug 26 2026 | 1M | $0.83 / $2.50 | Open 770B; coding, office work, research |
| qwen3.8-max, qwen3.8-max-0902, qwen3.8-flash-next | Alibaba | Aug 3 2026 | 1M | $2 / $6 | Best open multimodal; document and OCR leader |
| mimo-v2.6-pro, mimo-v2.6-flash | Xiaomi | Sep 22 2026 | 1M | $0.44 / $0.87 | Strongest open model for the money |
| muse-spark, muse-spark-1.1, -1.2, -1.3, -1.3-max | Meta | Apr-Sep 2026 | 1M | $1.25 / $4.25 | Multimodal agentic + coding |

Note: `-preview` means the maker still labels it a test build.

---

## Part 2 - Ranked by capability (best at top)

### A. Coding and software engineering

1. gpt-6-astra (max) - OpenAI's flagship; tops coding and computer-use tests
2. claude-opus-5.5 (max) - Anthropic's newest; leads agentic coding
3. claude-fable-5.1 (max) - Anthropic's most capable; ambitious multi-day coding
4. gpt-6.1-sol (max) - nearly matches Astra on agentic coding, one-fifth the price
5. claude-opus-5 (max) - SOTA on Frontier-Bench (43.3%) and near-Fable on CursorBench
6. gpt-5.6-sol (xhigh) - set the coding-agent index record (80) at launch
7. kimi-k3 (max) - open weights, competitive with Fable 5 on coding
8. gemini-4-argon (high) - Google's frontier; excelling at long-horizon code
9. muse-spark-1.3 (max) - Meta; #1 on a key agentic-work benchmark
10. gpt-6-sol (max) - cost-efficient high end, long-horizon work in real repos
11. grok-4.7 (xhigh) - built for coding; default model in xAI's coding tool
12. qwen3.8-max - open; top-3 on Terminal Bench 2.1 and SWE-Bench Pro
13. mimo-v2.6-pro - open; Terminal Bench 2.1 (89.9) beats Opus 5 (89.1)
14. gemini-3.8-flash (high) - best agentic coding Google has shipped
15. hy4-preview - open 770B; beats GLM 5.3 and Kimi K3 in Tencent's own blind test
16. claude-sonnet-5.5 (xhigh) - 70.6% on Terminal-Bench 4.0 (Sonnet 5 was 10.3%)
17. claude-opus-4.7 (high) - CursorBench ~70% vs 4.6's 58%
18. claude-opus-4-6 (high) - 2026's early coding leader
19. gemini-3.7-flash (high) - FrontierCode 43.6%, DeepSWE 65.3%
20. gemini-3.1-pro-preview - solid, older frontier
21. gemini-3-pro - legacy; use only if it is all you have

### B. Reasoning and knowledge work

1. gpt-6-astra (max) - "most intelligent model in the world" per OpenAI
2. claude-fable-5.1 (max) - Mythos-class deep reasoning
3. claude-opus-5.5 (max) - leads knowledge work too
4. gpt-6.1-sol (max) - near-Astra at a fraction of the cost
5. claude-opus-5 (max) - GDPval-AA 1861, top of that chart
6. gemini-4-argon (high) - deep multi-step long-horizon reasoning
7. gpt-5.6-sol (xhigh) - strong reasoning, cheaper
8. kimi-k3 (max) - frontier reasoning in open weights
9. muse-spark-1.3 (max) - Meta's top reasoning variant
10. gemini-3.8-flash (high) - HLE-Verified 54.9%
11. qwen3.8-max - strong on analysis + documents
12. mimo-v2.6-pro - top open model on the intelligence index (46)
13. grok-4.7 (xhigh) - better self-checking, longer documents
14. hy4-preview - good general reasoning, very cheap
15. claude-sonnet-5.5 (xhigh) - near-Opus on everyday work
16. gemini-3.1-pro-preview - older frontier
17. claude-opus-4.7 (high) - 2026 mid-year
18. claude-opus-4-6 (high) - 2026 early
19. gemini-3.7-flash (high) - workhorse
20. gemini-3-pro - legacy

### C. Computer use and autonomous agents

1. gpt-6-astra (max) - best-in-class computer use
2. claude-opus-5.5 (max) - strong agentic + safest in a sandbox
3. gpt-6.1-sol (max) - near-Astra on OSWorld 2.0
4. claude-fable-5.1 (max) - long autonomous sessions
5. muse-spark-1.3 (max) - #1 on a key agentic benchmark
6. claude-opus-5 (max) - strong long multi-step work
7. gemini-4-argon (high) - long-horizon autonomy
8. kimi-k3 (max) - sustains long engineering sessions with little oversight
9. gpt-5.6-sol (xhigh) - good agents, cheaper
10. grok-4.7 (xhigh) - agentic, Grok Build default
11. qwen3.8-max - good tool/agent use
12. mimo-v2.6-pro - OSWorld-Verified 82.0
13. gemini-3.8-flash (high) - Google agentic workhorse
14. claude-sonnet-5.5 (xhigh) - everyday agents
15. hy4-preview - tool-using workflows

### D. Vision and multimodal (images, video, audio, documents)

1. gemini-4-argon (high) - Google's native multimodal frontier
2. qwen3.8-max - best OCR / document / visual reasoning in tests
3. gemini-3.8-flash (high) - text, image, video, audio, PDF in one
4. gpt-6-astra (max) - image in, strong visual + computer use
5. mimo-v2.6-pro - true omni-modal (text, image, video, audio)
6. claude-opus-5.5 (max) / claude-fable-5.1 (max) - image input, strong charts + screenshots
7. muse-spark-1.3 (max) - image, video, audio, PDF
8. kimi-k3 (max) - natively multimodal
9. claude-opus-4.7 (high) - big vision upgrade over 4.6
10. gemini-3.1-pro-preview - strong but older

### E. Cybersecurity (defensive / dual-use)

These makers deliberately gate or weaken offensive cyber, so "best" is about defence.

1. gpt-6-astra (max) - OpenAI rates it Critical; gated behind trusted access
2. gpt-6.1-sol (max) - also Critical; ExploitBench 99.7% at max effort
3. gpt-5.6-sol (xhigh) - OpenAI's first "strongest cyber" model
4. gemini-4-argon (high) / gemini-3.8-flash-cyber - Google Fairwind programme
5. grok-4.7 (xhigh) - improves defender tasks; biosafety lead (LatchBio 62.4%)
6. claude-opus-5.5 (max) - Anthropic weakens risky cyber on purpose; best safeguards

### F. Math and science research

1. gpt-6-astra (max) - solved 10 human-level math problems, Lean-verified proofs
2. claude-fable-5.1 (max) / claude-opus-5.5 (max) - biology, drug design, science
3. gemini-4-argon (high) - built for deep professional + scientific work
4. kimi-k3 (max) - strong reasoning, cheap for the class
5. qwen3.8-max - wins PaperBench (reproducing research from scratch)
6. hy4-preview - strong on physics, math, molecular simulation

### G. Best value for this phone (quality per dollar)

Cheap and fast matters here, because every call goes out over the network.

1. mimo-v2.6-pro - top open model at ~$0.44 in / $0.87 out
2. gemini-3.8-flash (high) - near-frontier at $0.75 / $3.75
3. hy4-preview - open 770B at $0.83 / $2.50
4. gpt-6.1-sol (max) - near-Astra at $2 / $10
5. claude-sonnet-5.5 (high) - $2 / $10, fast
6. grok-4.7 (high) - $2 / $6
7. muse-spark-1.3 (xhigh) - $1.25 / $4.25
8. qwen3.8-max - $2 / $6, best documents
9. kimi-k3 - cheap flagship, open weights
10. gemini-3.7-flash (high) - $0.75 / $3.75, fine for everyday

### H. Raw power, price ignored

1. gpt-6-astra
2. claude-fable-5.1
3. claude-opus-5.5
4. gpt-6.1-sol
5. claude-opus-5
6. gemini-4-argon
7. gpt-5.6-sol
8. kimi-k3
9. muse-spark-1.3
10. gemini-3.8-flash
11. qwen3.8-max
12. mimo-v2.6-pro
13. grok-4.7
14. hy4-preview
15. claude-sonnet-5.5
16. claude-opus-4.7
17. claude-opus-4.6
18. gemini-3.7-flash
19. gemini-3.1-pro-preview
20. gemini-3-pro

---

## Part 3 - What this means on this phone

- The gateway's own table (the old `models-by-capability.md`) only split **coding** from
  **everything else**, and its top coding pick was `claude-opus-5.5-max`. The research above
  agrees the top is a small group (Astra, Opus 5.5, Fable 5.1, GPT-6.1 Sol) and adds the
  other skills (agent, vision, cyber, value) it could not rank.
- Cheapest good defaults for daily work here: **mimo-v2.6-pro**, **gemini-3.8-flash**,
  **gpt-6.1-sol**.
- Strongest all-rounders: **gpt-6-astra**, **claude-opus-5.5**, **claude-fable-5.1**.

## Sources

- Anthropic: anthropic.com/news/claude-opus-5 , /claude-opus-5-5 , /news/claude-opus-4-7 ,
  /news/claude-opus-4-6 , /claude-sonnet-5-5 , platform.claude.com/docs/en/models/overview
- OpenAI: openai.com/index/gpt-6-astra , /introducing-gpt-6-1-sol , /introducing-gpt-6-sol-and-luna ,
  /previewing-gpt-5-6-sol , deploymentsafety.openai.com/gpt-6-astra
- Google: blog.google (gemini-3-8-flash, gemini-3-7-flash, gemini-4-argon, gemini-3-1-pro),
  deepmind.google/models , ai.google.dev/gemini-api/docs/models
- xAI: x.ai/news/grok-4-7 , docs.x.ai/developers/grok-4-7 , media.x.ai Grok 4.7 model card
- Moonshot: moonshot.ai , fortune.com/2026/07/16 moonshot-kimi-k3
- Tencent: tencent.com/tencent-releases-and-open-sources-tencent-hy4-preview ,
  github.com/Tencent-Hunyuan/Hy4-preview
- Alibaba: qwen.ai/blog?id=qwen3.8 , vals.ai/models/alibaba_qwen3.8-max
- Xiaomi: mimo.xiaomi.com/mimo-v2-6 , artificialanalysis.ai/models/mimo-v2-6-pro
- Meta: dev.meta.ai/models/muse-spark , ai.meta.com/blog/introducing-muse-spark-msl ,
  artificialanalysis.ai/models/releases/muse-spark-1-3
