# Levea — Agentic Video Production Platform

> **The professional agentic production environment for editable video.** Levea turns natural-language creative direction, transcripts, and source media into fully structured, editable video projects, executes edits through a deterministic production harness, verifies the result, and renders delivery files.

[![npm](https://img.shields.io/npm/v/levea-mcp-server?label=npm%20levea-mcp-server)](https://www.npmjs.com/package/levea-mcp-server)
[![MCP Registry](https://img.shields.io/badge/MCP%20Registry-levea--mcp--server-orange)](https://registry.modelcontextprotocol.io/v0/servers?search=levea-mcp-server)
[![ClawHub Plugin](https://img.shields.io/badge/ClawHub-Plugin-blue)](https://clawhub.ai/plugins/openclaw-ai-video-editor)
[![ClawHub Skill](https://img.shields.io/badge/ClawHub-Skill-orange)](https://clawhub.ai/skills/levea-ai-video-editor)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)

> **Beta Notice:** Agentic edits can make mistakes. Always preview every output before publishing. For production-safe and high-impact workflows, use `requirePlanApproval: true` to halt execution after the planning phase and inspect the proposed edit list.

---

## ⚡ The Levea Philosophy: Prompt-to-Project

Unlike traditional one-shot AI video generators that output locked, un-editable pixels, Levea maintains a fully structured, multi-layer **project and timeline (Scene IR)** containing assets, text, masks, audio, and brand kits. 

We use probabilistic **Frontier LLMs solely for planning, semantic analysis, and parameter parsing**. The actual layout, timing, audio cleanup, face tracking, and composition are executed by a high-performance **deterministic video-production harness**. Generative media models are optional, modular assets; they do not own the project state.

---

## 🏗️ Platform Architecture

Levea separates probabilistic creative reasoning from deterministic project execution:

```text
                     Creative Intent + Source Media
                                  │
                                  ▼
               Probabilistic Multimodal Intelligence
                          (Frontier Models)
                                  │
                                  ▼
                Typed Edit Graph / Media IR
                        (Scene Graph DAG)
                                  │
                                  ▼
               Deterministic Video-Production Harness
                   ├── Timeline and Scene Graph
                   ├── Media Operators
                   ├── Caption and Layout Engine
                   ├── Animation and Motion System
                   ├── Generative Media Adapters
                   ├── Composition and Asset Execution
                   │   ├── Remotion — cards, charts, diagrams and editorial compositions
                   │   ├── Vulkan/WebGPU — captions, primitives, effects and final compositing
                   │   ├── Lottie — verified authored vector assets
                   │   ├── External Rive — explicitly supplied interactive/vector assets
                   │   └── Omni/Veo — generated supporting media
                   ├── Validators
                   ├── Project Versioning
                   └── Export Pipeline
                                  │
                                  ▼
                  Verification → Bounded Repair → Editable Scene
                                                    ├── Return
                                                    ├── Queue Media Work
                                                    └── Optional Export
```

### 1. Multimodal planning
The planning layer interprets user prompts, transcripts, visual references, and timeline boundaries. It compiles raw natural language into a highly optimized, typed **Workflow DAG** of sequential and parallel editing operations. The hosted planner currently utilizes Gemini through Google AI and Vertex AI.

### 2. Workflow DAG and Scene IR (Intermediate Representation)
These represent the dual brain-body architecture of Levea:
- The **Workflow DAG** maps the editing tasks, dependencies, gating, verifiers, and asset-generation jobs.
- The **Scene IR** is the serializable media schema representing the complete timeline: canvas dimensions, tracks, layers, custom transitions, effects, and assets.

Separating intent from execution makes edits fully inspectable, repeatable, and independently repairable without transferring project ownership to an LLM.

### 3. Deterministic Production Harness
Typed production operators execute the plan against the scene graph. The native renderer compiles Scene compositions through native Vulkan and browser WebGPU paths, subsequently compositing them with verified composition assets produced by Remotion (which handles rich editorial layouts, cards, and diagrams).

### 4. Versioning and Export
All revisions are saved in a durable, linear undo/redo history backed by immutable scene payloads, preventing state corruption during multi-step iterations. Export is optional—Levea can return the updated editable scene, queue asset rendering, deliver an MP4, or bundle assets for multi-platform delivery.

---

## 🛠️ Dual-Path Quickstart (Get Started in 60 Seconds)

Whether you are an AI developer looking to integrate automated editing into your agent loops, or a content creator building an automated faceless channel, Levea has a native path for you.

### 🧑‍💻 Path A: For Developers & AI Engineers (The MCP Route)

Expose Levea as a client-side Model Context Protocol (MCP) server stdio wrapper (`levea-mcp-server`) in your favorite AI editors (Cursor, Cline, Windsurf, or Claude Desktop).

#### 1. Get an API Key
Sign up at [studio.livecore.ai](https://studio.livecore.ai/) and generate a Levea API key.

#### 2. Register the MCP Server
Add the following configuration block to your editor's MCP settings:

```jsonc
{
  "mcpServers": {
    "levea": {
      "command": "npx",
      "args": ["-y", "levea-mcp-server"],
      "env": {
        "LEVEA_API_URL": "https://api.livecore.ai",
        "LEVEA_API_KEY": "your-key-from-studio.livecore.ai"
      }
    }
  }
}
```

*Note: `LEVEA_API_URL` should point to the bare host domain `https://api.livecore.ai`. The client-side wrapper handles endpoint appending automatically.*

| Client | Setup Commands / Instructions |
| --- | --- |
| **Cursor & Windsurf** | Go to Settings -> Features -> MCP -> Add New MCP Server. Set type to `command` and insert the config above. |
| **Cline** | Open settings, scroll to MCP, and add the config to the Cline MCP settings file. |
| **Claude Code** | Run: `claude mcp add levea -e LEVEA_API_URL=https://api.livecore.ai -e LEVEA_API_KEY=... -- npx -y levea-mcp-server` |
| **Claude Desktop** | Add the server block to `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows). |

---

### 🎨 Path B: For Creators & Marketers (The OpenClaw Route)

Deploy Levea as an autonomous editing agent directly inside the OpenClaw workspace.

1. **Install the Plugin:** Search for `openclaw-ai-video-editor` on ClawHub and install it to gain system-level execution, asset upload UI, and rendering support.
2. **Install the Skill:** Subscribe to `levea-ai-video-editor` on ClawHub Skills directory to give your agent professional editorial taste, system instructions, and pacing rules.
3. **Run your first edit:**
   > *"Review my vertical video, remove the opening silence, auto-apply Hormozi-style captions with highlighted keywords, add subtle background music, and export the Reels-ready MP4."*

---

## 🧩 OpenClaw Integration: Plugin vs. Skill

Levea is built for modular agent platforms. When deploying Levea inside platforms like OpenClaw or Hermes, we split execution capabilities from cognitive strategies:

- **The Plugin (`openclaw-ai-video-editor`):** Acts as the **physical body**. It exposes the core MCP tool surface, registers tool schemas, manages API keys, sets up SSE progress pipelines, and handles physical file uploads and rendering hooks.
- **The Skill (`levea-ai-video-editor`):** Acts as the **cognitive mind**. It is a prompt-engineered, context-aware instruction set (system prompts and few-shot creative templates) that teaches the agent how to act as a professional director—enforcing brand rules, safe zones, timing pacing, and layout aesthetics.

*We recommend installing **both** to unlock the full power of autonomous editing with a professional finish.*

---

## 🎬 What You Can Ask For (Creator & Dev Use Cases)

Levea supports a wide range of natural-language production instructions:

| Use Case / Request | Typical Autonomous Production Path | Organic Keywords |
| --- | --- | --- |
| **Faceless Channel Generator** | Segment transcript, plan layout, overlay B-roll, generate AI background music, and render. | `faceless-video`, `auto-reels`, `short-form-video` |
| **CapCut Auto-Cap Alternative** | Run whisper transcription, highlight keywords, style fonts, align timing, and apply animation presets. | `auto-captions`, `video-subtitles`, `kinetic-typography` |
| **Shorts & Reels Highlights** | Extract high-engagement hooks, crop canvas to vertical 9:16 safe zones, and apply motion graphics. | `viral-clips`, `clip-generator`, `tiktok-video`, `youtube-shorts` |
| **Corporate Interview Polish** | Cut long silence gaps, bleep profanity, apply color grades, and add lower third speaker graphics. | `silence-removal`, `audio-cleanup`, `lower-thirds` |
| **Green Screen & Backdrop Swap** | Isolate speaker matte, layer background photo/video, align depth tracks, and composite. | `chroma-key`, `green-screen`, `background-removal` |
| **Multi-Cam Active Speaker Cuts** | Synchronize dual camera angles, run diarization, and automatically cut to the active speaker. | `multi-cam-sync`, `active-speaker`, `video-automation` |

---

## ⚙️ Robust Verification & Repair Containment

To ensure that AI planning errors never result in broken compositions or corrupt files, Levea operates a closed-loop verification and repair containment pipeline.

```text
User correction
      ↓
Semantic-node reference resolution
      ↓
Typed repair patch
      ↓
Repair-policy validation
      ↓
Affected-subgraph invalidation
      ↓
Partial recompilation and execution
      ↓
Verification
      ↓
Atomic replacement of prior version
```

- **Internal Verifier-Driven Repair:** Levea currently supports verifier-driven bounded repair for supported typed failures. Verifier failures identify the unsatisfied invariant, repair begins from the last verified scene revision, and every repaired result is verified again before it is committed.
- **User-Directed Semantic Repair:** User-directed repair operates at the level of user-visible semantic nodes rather than low-level infrastructure tasks. Where a node type supports natural-language repair, Levea resolves the user’s reference—such as “the second chart,” “the last title,” or “the graphic after the pricing section”—to a stable semantic node and compiles the requested change into a typed patch.
- **Subgraph Invalidation:** The repair system then determines which dependent planning, asset, composition, rendering, and verification nodes are affected. Only that subgraph is invalidated and executed again; unrelated verified work is preserved. User language never directly mutates arbitrary scene JSON or internal execution tasks.
- **Bounded Budgets:** Natural-language node repair is available only for semantic node types that expose stable identity, editable fields, and a repair policy. All repair attempts remain bounded by action, attempt, cost, and time budgets.
- **Asynchronous Completion Verification:** A workflow may return a pending or partial result while optional media jobs continue. A result is considered fully verified only after all required artifact jobs complete and their outputs pass verification.

---

## 📂 Capability Status

Availability of specific tracks varies by deployment, active model tiers, and account quotas.

### Supported Production Paths
- **Project and timeline state:** Scene projects, layer insertion/updates, grouping, trimming, splitting, sequencing, retiming, track-relative alignment, and linear undo/redo.
- **Captions and motion graphics:** automatic captions, word timing, keyword emphasis, caption templates, lower thirds, title cards, charts, counters, and diagrams through the motion-graphics composition path (using verified Remotion compositions or supported native fallbacks), verified Lottie assets, explicitly supplied external Rive assets, and supported procedural animation.
- **Layout and perception:** scene and shot analysis, face detection, active-speaker workflows, on-screen text-region detection, safe zones, and explicit-region tracking or masking.
- **Compositing:** chroma key, masks, blend modes, adjustment layers, alpha-matte background replacement, and GPU effects.
- **Audio:** silence and filler-word cleanup, word-level muting, crossfades, EQ, denoise, loudness normalization, and speech-aware ducking.
- **Verification:** typed task contracts, structural validation, perceptual checks, requirement tracking, bounded repair, and partial-success reporting.

### Model- or Deployment-Dependent
- Generated video, images, B-roll, music, sound effects, voiceover, and voice cloning.
- Neural alpha matting and background replacement quality.
- OCR **recognition** of visible text.

---

## 📂 MCP Tool Surface

The MCP server exposes one high-level editing entry point plus typed management and polling tools:

| Group | Tools | Description |
| --- | --- | --- |
| **Edit** | `autonomous_edit`, `autonomous_edit_streaming`, `queue_edit` | Single-entry edit prompts, SSE progress streaming, and asynchronous queuing. |
| **Job Polling** | `check_job_status`, `check_task_status`, `get_active_task` | Track rendering, B-roll generation, tracking status, and active tasks. |
| **Caption Templates** | `list_caption_templates`, `apply_caption_template`, `save_caption_template` | CRUD operations for 41+ styling templates (Hormozi, Minimal-Pro, typewriter...). |
| **Brand Kits** | `list_brand_kits`, `get_brand_kit`, `create_brand_kit`, `update_brand_kit` | Manage brand colors, fonts, logos, speaker voice clones, and grading rules. |
| **Projects** | `list_projects`, `get_project`, `create_project` | Project workspace management. |
| **Assets** | `asset_upload_url`, `list_assets`, `transcribe_asset` | Request signed upload URLs, list assets, and request fast Whisper transcriptions. |
| **Diagnostics** | `editor_health` | Unauthenticated network sanity probe. |

---

## 🔮 Organic SEO FAQ for Developers & Creators

#### How does Levea compare to generic video generators like Sora, Veo, or Runway?
Generic generative video models (Sora, Runway, Veo) output raw, locked pixels. You cannot edit a layer, adjust caption typography, swap background music, or correct a word timing afterward. Levea is a **full-featured timeline editor** that builds structured project layers. It uses generative models (like Omni/Veo/Imagen) only as optional asset generation plugins, keeping your editing pipeline fully editable, inspectable, and deterministic.

#### Is Levea safe to use in enterprise productions?
Yes. Every mutation runs inside a secure, gated environment (`GatedExecutor`). High-level action contracts ensure that LLMs cannot inject arbitrary mutations or bypass security profiles. If you configure `requirePlanApproval: true`, the system will halt and present the creative plan to your team for approval before executing any edits or rendering assets.

#### What coding and agent environments does Levea support?
Levea integrates natively with **Model Context Protocol (MCP)** hosts like Claude Desktop, Cursor, Cline, Windsurf, and Claude Code. For standalone agent systems, Levea exposes structured packages for **OpenClaw** and **Hermes**.

#### Can I use custom brand fonts, logos, and specific caption templates?
Absolutely. Using our **Brand Kits API and tools**, you can declare custom typography scales, palette hex codes, logo image references, and custom voice prints. The planning model reads these rules and automatically enforces them across the timeline during the composition pass.

---

## 🔗 Links & Resources

- **Levea Studio and API Keys:** [studio.livecore.ai](https://studio.livecore.ai/)
- **npm MCP Server Wrapper:** [`levea-mcp-server`](https://www.npmjs.com/package/levea-mcp-server)
- **MCP Registry:** [`io.github.brajendrak00068/levea-mcp-server`](https://registry.modelcontextprotocol.io/v0/servers?search=levea-mcp-server)
- **OpenClaw Plugin Page:** [`openclaw-ai-video-editor`](https://clawhub.ai/plugins/openclaw-ai-video-editor)
- **OpenClaw Skill Page:** [`levea-ai-video-editor`](https://clawhub.ai/skills/levea-ai-video-editor)
- **Detailed Agent Integration Guide:** [AGENTS.md](./AGENTS.md)
- **MCP Folder Documentation:** [mcp-server/README.md](./mcp-server/README.md)

**Support & Contact:** `brajendrak00068@gmail.com`

---

## License

[MIT](./LICENSE)
