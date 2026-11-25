id:: 6b062bec-9123-4150-ae43-e7607253ef1e

- # Harness Engineering vs Prompt Engineering vs Context Engineering
  - [at Atlan](https://atlan.com/know/harness-engineering-vs-prompt-engineering/). Emily Winks. Sep 21, 2026.
- ## Summary
  - Prompt engineering controls what you ask the model. Context engineering controls what the model sees -- the delicate art of filling the context window with just the right information. Harness engineering controls how the entire agent environment operates: tools, memory, constraints, and feedback loops. The disciplines are nested, not competing, and in enterprise settings they often depend on governed data foundations such as the kind Atlan provides.
  - Core Components:
    - Prompt engineering -- message-level instruction design (2022-2024)
    - Context engineering -- session-level information architecture (2025)
    - Harness engineering -- system-level agent environment design (2026+)
- ## Preface
  - Prompt engineering got AI teams off the ground. Context engineering made agents smarter. But neither solved production reliability, and that is where harness engineering enters.
  - The three-tier evolution maps to a progression in where failure lives:
    - **Prompt engineering**. Failure mode: poorly worded instructions produced inconsistent outputs. Solution: craft better instructions.
    - **Context engineering**. Failure mode: agents confidently reasoned over the wrong information. Solution: architect what goes into the context window.
    - **Harness engineering**. Failure mode: well-instructed agents with good context still failed unpredictably at scale. Solution: engineer the entire operational environment.
  - Each discipline contains the one before it. The shift at each boundary is not a replacement; it is an expansion of what counts as the controllable surface.
    - | Attribute | Detail |
      | ---- | ---- |
      | Topic | Harness engineering vs prompt engineering vs context engineering |
      | Search intent | Comparison / informational |
      | Evolution timeline | Prompt (2022–2024) → Context (2025) → Harness (2026+) |
      | Core distinction | Nested layers: message-level → context-level → system-level |
      | What prompt engineering controls | Instructions and prompt structure |
      | What context engineering controls | The information supplied to the model |
      | What harness engineering controls | Tools, memory, constraints, and feedback loops |
      | Enterprise risk across all three | Untrusted or poorly governed data |
- ## How do prompt, context, and harness engineering compare?
  - Prompt, context, and harness engineering differ across eight dimensions: what each controls, abstraction level, emergence period, failure mode, tooling, use case, origin, and defining quote. The table compares prompt engineering at the message level, context engineering at the context-window level, and harness engineering at the system level.
    - | Dimension | Prompt Engineering | Context Engineering | Harness Engineering |
      | ---- | ---- | ---- |
      | What it controls | Single instructions | Context window content | Full agent environment |
      | Level of abstraction | Message-level | Session-level | System-level |
      | Era | 2022-2024 | 2025 | 2026+ |
      | Primary failure mode | Poorly worded instructions | Wrong or incomplete context | Data layer failures |
      | Tools needed | None beyond LLM | RAG, retrieval systems | Orchestration + data governance |
      | Best for | Simple queries | Knowledge-grounded answers | Autonomous, multi-step agents |
      | Who coined it | Community/informal | Karpathy, Lutke (2025) | Martin Fowler (2026) |
      | Key practitioner quote | “Use better words” | “Context is RAM, model is CPU” | “Agent = Model + Harness” |
- ## Prompt engineering vs context engineering vs harness engineering: what’s the difference?
  - The three disciplines are nested layers, not competing methodologies. Think of it as a computing stack: the model is the CPU, context is the RAM, and the harness is the operating system. Each layer assumes the previous one is in place.
  - **Prompt engineering** operates at the message level. It controls what you type, how you structure instructions, and how you frame a question to elicit a useful response from a large language model. It is the smallest unit of control.
  - **Context engineering** operates at the session level. It controls what information the model sees across an entire task: retrieved documents, memory fragments, tool call outputs, conversation history, and schema definitions. Andrej Karpathy defined it as “the delicate art and science of filling the context window with just the right information for the next step.”
  - **Harness engineering** operates at the system level. Martin Fowler’s formulation is the clearest: “Agent = Model + Harness.” The harness is everything in an agent except the model itself: guides, sensors, toolchain management, memory systems, and lifecycle management.
  - ### How did prompt engineering evolve into context and harness engineering?
    - Between 2022 and 2024, prompt engineering was the primary lever for improving LLM output quality. It worked well for single-turn, bounded tasks.
    - In 2025, Karpathy’s definition crystallized context engineering as a distinct discipline. Tobi Lutke (Shopify) described it as “the art of providing all the context for the task to be plausibly solvable by the LLM.” Simon Willison captured why the rename mattered: prompt engineering had been “redefined to mean typing prompts full of stupid hacks into a chatbot.” The term was diluted by overuse, but the practice of carefully crafting instructions was not diminished.
    - In 2026, harness engineering emerged as agents operating at scale revealed problems that neither prompt design nor context architecture alone could solve. The naming came with a formulation that stuck: “Agent = Model + Harness.”
  - ### Why do teams confuse prompt, context, and harness engineering?
    - The three disciplines are nested, not competing, which means practitioners solving a harness problem are also doing context engineering and prompt engineering simultaneously. Martin Fowler’s framing complicates this further: he positions harness engineering as a specific form of context engineering for coding agents, not above it. That definitional instability is real.
    - The more consequential confusion: practitioners who fix the harness architecture but not the data it reads are solving the wrong 20% of the problem. Understanding what an agent harness actually contains helps clarify which failure mode you are actually facing.
- ## What is prompt engineering?
  - Prompt engineering is the discipline of designing instructions that elicit accurate, useful responses from large language models. It operates at the message level: the user’s input, system prompt, and turn-by-turn instructions within a single interaction.
  - The primary techniques are well established:
    - **Zero-shot prompting**: asking the model directly, without examples, relying on pre-trained knowledge
    - **Few-shot examples**: providing sample inputs and outputs to calibrate model behavior without fine-tuning
    - **Chain-of-thought reasoning**: prompts that elicit step-by-step reasoning to improve accuracy on complex tasks
    - **Role assignment**: instructions that define the model’s context, expertise, and behavioral constraints for a session
    - **Output format directives**: instructions that control response structure (JSON, markdown, bullet lists) for downstream parsing
  - ### Why it mattered, and why it reached its limits
    - Between 2022 and 2024, prompt engineering was the fastest way to improve LLM output quality. For single-turn or short-session tasks (writing, classification, extraction) it delivered meaningful gains quickly.
    - The gains were capped at the instruction layer. Prompt engineering for data retrieval helped teams extract structured output from raw data, but it could not prompt its way to reliable multi-step agent behavior. The failure mode was not poor wording. It was that the unit of control was too small. Once agents needed to reason over retrieved information, manage state across steps, and call external tools, a better question was not sufficient.
  - ### Where it still applies
    - Prompt engineering is not dead. It is a contained, necessary discipline inside every context engineering and harness engineering system. Every context-building pipeline still produces prompts; every harness still issues instructions. The difference is that prompt engineering is no longer the primary lever; it is a component inside a larger architecture.
    - Teams that treat prompt tuning as the answer to multi-step agent failures are applying a message-level fix to a system-level problem.
- ## What is context engineering?
  - Context engineering is the discipline of deciding what information populates the context window at inference time. The unit of control shifts from the individual message to the entire session: all inputs the model sees in a single task, including retrieved documents, memory fragments, tool outputs, conversation history, and schema definitions.
  - Karpathy’s definition is the most-cited: “the delicate art and science of filling the context window with just the right information for the next step.” The key word is “right”: not just any information, but information that is relevant, timely, and trustworthy.
  - ### Core components
    - Context engineering involves five interconnected systems:
      - **Retrieval pipeline**: the mechanism (RAG, vector search, keyword search) that selects which documents or data chunks enter the context window
      - **Memory system**: short-term conversation history, long-term episodic memory, and semantic memory that persists across sessions
      - **Context compression**: techniques (summarization, filtering, prioritization) that fit more relevant information into a fixed token budget
      - **Dynamic tool result injection**: real-time injection of tool call outputs (API results, database queries, search snippets) into the active context
      - **System context layer**: metadata, schema definitions, business glossary terms, and ontology anchors that give the model semantic grounding beyond raw text
  - ### Why context engineering is the dominant discipline in 2025-2026
    - The shift from prompt to context engineering coincided with the rise of autonomous agents: systems that run multi-step tasks without human input at each step. Jeff Huber, CEO of Chroma, framed it directly: “Context engineering is the job today. If you’re looking to build a production system, that is the job.”
    - Gartner identified [context engineering](https://atlan.com/know/agent-context-layer/) as a critical skill for successful AI-enabled processes. Enterprises are now hiring context designers alongside ML engineers, reflecting how central the discipline has become to production AI deployment.
  - ### What context engineering alone cannot solve
    - In practice, most context engineering implementations operate without adequate data governance; they treat retrieved information as trustworthy by default. In enterprise environments, this assumption fails constantly.
    - Stale lineage, uncertified tables, schema drift, and conflicting business definitions enter the context window and produce reliable-sounding hallucinations. The agent is not making a reasoning error. It is reasoning correctly over incorrect inputs. That is a data problem, not a model problem. A rigorous context engineering system would include data certification as part of its information selection criteria, but most implementations skip this step entirely.