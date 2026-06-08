# 🌙 Silkworm Framework

> **Autonomous Embodied VLA Agent Framework**
> A heavily modified, specialized fork of the Hermes Agent framework. Designed for raw, generalized interaction rather than API-specific wrappers.

> ⚠️ **Disclaimer**
> *This project is experimental and written ENTIRELY with the help of AI.*
> *This means that errors in the code and architecture are still possible, and the project should be tested with caution before being used on a real public server.*

---

## 👁️ Core Vision: General Embodied Agent (Raw I/O)

The defining characteristic of the **Silkworm Framework** is its absolute rejection of high-level API wrappers (like Mineflayer). We are building a low-level, high-performance core (C++ / TypeScript) that interacts with any environment exactly as a human does:
- **Visual Input:** Interpreting raw screen buffers and pixel data natively.
- **Action Tokens to Raw Output:** The Reflex SLM outputs semantic **Action Tokens** (e.g., `<ACTION_MOVE_FWD>` or `{"cmd": "hit"}`). The custom C++ core instantly intercepts these tokens and translates them into OS-level hardware keystrokes (`W`, `A`, `S`, `D`, `Mouse Delta`).

This architectural shift transforms Oneiro from a mere "game bot" into a true **General Embodied Agent**. Whether it's Minecraft, a terminal, or a 3D engine, Silkworm plays it by looking at the screen and pressing buttons.

---

## 🧠 Cognitive Engine: Hermes Foundation & MoA

We leverage the **Hermes Agent framework (by Nous Research)** as our cognitive foundation, wrapping our C++/TS hardware core in its powerful persistent learning loop.

To achieve both high-frequency reaction times and complex long-term reasoning, Silkworm splits cognitive load via a **Mixture of Agents (MoA)**:

### 1. The Reflex Agent (Motor Cortex)
- **Models:** `Oneiro MC Lite` (e.g., Gemma 1B - 3B)
- **Role:** A lightweight SLM running locally at high frequency (simulating FPS).
- **Function:** Handles immediate survival, micro-movements, combat, and parkour. It interprets immediate visual/state tokens and fires raw keystrokes in real-time. It does not think about the meaning of life; it thinks about dodging the creeper.

### 2. The Planner Agent (Prefrontal Cortex)
- **Models:** `Oneiro MC` (e.g., Gemma 4 12B) or `Oneiro MC Medium` (e.g., Gemma 4 4B)
- **Role:** A heavier, deeper reasoning model that activates periodically (e.g., once every 30-60 seconds).
- **Function:** Takes high-resolution snapshots of the world, analyzes the environment, and dictates macro-goals. It uses strict **Function Calling (JSON)** to push directives down to the Reflex Agent (e.g., `{"objective": "mine_iron", "urgency": "high"}`).

### 3. The Social Agent (Voice & Emotion)
- **Models:** Cloud-based multimodal models (e.g., Gemini Flash Live)
- **Role:** Handles real-time voice processing and emotional synthesis via WebSockets.
- **Function:** Listens to raw audio streams, interprets the tone of the player, and generates real-time spoken responses. Totally decoupled from movement logic to prevent context overflow.

---

## 🛡️ "Friend or Foe" System (152-FZ Compliance)

To comply with strict data localization and privacy laws (e.g., Federal Law No. 152-FZ regarding biometric data), Silkworm **does not use voice biometrics** to recognize who is speaking.

Instead, it uses a **Vector Injection** system:
1. The voice chat plugin (e.g., Simple Voice Chat) transmits the speaker's in-game `UUID` to the backend.
2. The backend cross-references the UUID with the server's reputation database (HorniDB).
3. The system injects a hidden system tag directly into the Social Agent's prompt.
   *Example: `[System: Interlocutor - Cokeef. Status: Foe. Tone: Aggressive]`*
4. The Social Agent naturally adapts its tone and behavior based on this injected context.

---

## 🗄️ Memory & Persistent Learning

Inherited and enhanced from the base Hermes architecture, Silkworm features a persistent **Closed Learning Loop**:
- **SQLite + FTS5:** Replaces unreliable vector-only RAG for session history, ensuring lightning-fast and accurate recall of past events, player interactions, and grudges.
- **Skill Generation:** When the Planner Agent successfully solves a complex new problem, it automatically generates a reusable "Skill" (Markdown format), allowing it to bypass heavy reasoning steps in the future.
- **Context Preservation:** Oneiro remembers who built what, who attacked whom, and maintains a consistent personality across server restarts.

---

## 🚀 The Oneiro Lineup (Running on Silkworm)

We are building a scalable lineup of agents to suit different deployment needs:
- **Oneiro MC Lite:** (~1B-2B parameters) Designed to run entirely locally on player hardware.
- **Oneiro MC Medium:** (e.g. Gemma 4 E2B/E4B) Balanced for self-hosting or light server loads.
- **Oneiro MC (Flagship):** (e.g., Gemma 4 12B) The primary server-side intellect, driving the most complex narrative characters in the Horni universe.

*(Note: **Silkworm** is the underlying engine. **Oneiro** is the specific neuro-product / AI personality that runs on top of this framework in the Horni universe).*

