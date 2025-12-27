# Multi-Agent-Debate
🧠 LangGraph Structured Debate System (CLI)

A LangGraph-based multi-agent debate framework that simulates a structured, rule-enforced debate between two AI agents with memory control, turn validation, logging, and final adjudication.

The system runs exactly 8 rounds (4 turns per agent), strictly alternates speakers, prevents repeated arguments, maintains structured debate memory, and concludes with a JudgeNode that summarizes the debate and declares a winner with justification.

✨ Features

✅ Exactly 8 rounds (4 per agent, alternating)

🔁 Strict turn enforcement (out-of-turn calls rejected)

🧠 Memory slicing (agents receive only relevant context)

🚫 Duplicate argument detection (string + lightweight semantic checks)

🧩 Logical coherence checks (contradictions, topic drift)

⚖️ JudgeNode with summary + winner declaration

📜 Full logging (JSON logs with timestamps)

🖥️ Clean CLI interface

🔁 Deterministic runs via optional random seed

🗺️ Visual DAG diagram (Graphviz)

🏗️ Architecture Overview
Core Workflow Nodes
Node	Responsibility
UserInputNode	Accepts and validates debate topic
CoordinatorNode	Enforces turn order and round count
AgentA	Persona-driven debater (e.g., Scientist)
AgentB	Persona-driven debater (e.g., Philosopher)
MemoryNode	Stores structured debate memory and supplies slices
JudgeNode	Reviews debate, summarizes, and declares winner
LoggerNode	Logs all events, states, and transitions
🔄 Debate Flow (High Level)
UserInput
   ↓
Coordinator → AgentA → Memory
   ↓              ↑
Coordinator → AgentB → Memory
   ↓
(repeat until 8 rounds)
   ↓
JudgeNode
   ↓
Final Output + Logs

🧬 Memory Structure

All debate state is stored explicitly as JSON:

{
  "turns": [
    {
      "round": 1,
      "agent": "Scientist",
      "text": "AI systems in healthcare require strict oversight.",
      "meta": {
        "coherence_score": 0.92,
        "flags": []
      }
    }
  ],
  "summary": "Debate focused on regulation vs innovation."
}


Agents receive only filtered memory slices, not the full transcript.

🖥️ CLI Usage
Run the Debate
python run_debate.py

Example Session
Enter topic for debate: Should AI be regulated like medicine?

Starting debate between Scientist and Philosopher...

[Round 1] Scientist:
AI must be regulated due to its increasing use in safety-critical domains.

[Round 2] Philosopher:
Overregulation risks limiting intellectual freedom and innovation.

...

[Round 8] Philosopher:
History shows excessive regulation often delays societal progress.

[Judge] Summary:
The debate explored safety, innovation, ethics, and historical precedent.

[Judge] Winner: Scientist
Reason:
Provided consistent, risk-based arguments aligned with public safety principles.

⚙️ CLI Options
Flag	Description
--seed <int>	Deterministic agent behavior
--log-path <path>	Custom log file location
--persona-config <file>	Load custom agent personas
--graph-output <png/svg>	Export DAG diagram

Example:

python run_debate.py --seed 42 --log-path logs/debate_42.json

🧾 Logging

Format: JSON Lines (one event per line)

Includes:

Node inputs & outputs

State transitions

Memory snapshots

Coherence warnings

Final verdict

Example Log Entry
{
  "timestamp": "2025-01-02T10:31:45Z",
  "node": "AgentA",
  "round": 3,
  "event": "argument_generated",
  "content": "Unchecked AI poses systemic societal risks."
}

🗺️ DAG Diagram

A visual representation of the LangGraph workflow is generated using Graphviz.

python generate_dag.py


Output:

debate_graph.svg or debate_graph.png

🧪 Tests & Validation
Covered Checks

✅ Turn order enforcement

✅ Exactly 8 rounds

✅ Duplicate argument rejection

✅ Memory updates after each turn

✅ Judge output format & justification

✅ Deterministic replay using seed

Run Tests
pytest tests/

📁 Project Structure
.
├── run_debate.py
├── config.yaml
├── personas/
│   ├── scientist.txt
│   └── philosopher.txt
├── nodes/
│   ├── user_input.py
│   ├── coordinator.py
│   ├── agent_a.py
│   ├── agent_b.py
│   ├── memory.py
│   ├── judge.py
│   └── logger.py
├── graph/
│   └── build_graph.py
├── logs/
├── tests/
│   └── test_debate_flow.py
└── README.md

🔧 Configuration

Minimal configuration via config.yaml or CLI flags:

agents:
  agent_a: Scientist
  agent_b: Philosopher

rounds: 8
log_format: jsonl
coherence_threshold: 0.65

🚀 Future Enhancements

Semantic similarity via embeddings

Multi-judge voting

Streaming CLI output

Multi-agent (>2) debates

Web UI frontend
