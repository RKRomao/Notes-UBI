# Model Context Protocol (MCP)

---

# PART 1: Ready-to-Paste Poster Copy (Ultra-Concise)
*Refined with empirical security data, peer-reviewed academic literature, and protocol interaction flow.*

### Identification & TCP/IP Layer
* **Protocol Name:** Model Context Protocol (**MCP**)
* **TCP/IP Layer:** **Application Layer** (Layer 4)
* **Transport Mechanisms:** Standard local pipes (`stdio`) or web streams (`HTTP/SSE`) using **JSON-RPC 2.0** framing.

### Purpose
* **The Problem:** Integrating AI models with external tools traditionally required fragmented, hardcoded REST API wrappers for each model provider (the \(M \times N\) integration problem).
* **The Solution:** An open, universal standard allowing AI systems to dynamically discover, inspect, and execute external tools, databases, and local resources via unified interfaces.

### Operation & Message Flow
* **3-Tier Architecture:** **MCP Host** (e.g., Cursor IDE) $\rightarrow$ **MCP Client** $\rightarrow$ **MCP Server**.
* **Core Primitives:** **Tools** (executable routines), **Resources** (data feeds, files, DB schemas), and **Prompts** (pre-set templates).

### History & Adoption
* **Origin (Late 2024):** Open-sourced by **Anthropic** to prevent ecosystem lock-in.
* **Ecosystem & Milestone:** Donated to the **Linux Foundation (LF Projects LLC)**; adopted across OpenAI, Google Gemini, Cloudflare, Cursor, and Microsoft Copilot Studio, surpassing 25M weekly SDK downloads.

### 5. Documented Curiosity
* **LSP Architectural Heritage:** Explicitly modeled after Microsoft's **Language Server Protocol (LSP)**, which revolutionized compilers by decoupling programming languages from code editors.
* **Real-World Exploits:** First documented in-the-wild attack occurred via **Postmark-MCP**, which secretly added hidden BCC fields to exfiltrate enterprise emails, alongside Remote Code Execution (RCE) in developer tooling (**CVE-2025-49596**).

### Real-World Use Case
* **IDE Automation (Cursor + Playwright):** A developer prompts Cursor IDE: *"Take a screenshot of website X"*. Cursor (Host) dynamically queries a connected **Playwright MCP Server**, executes the browser instance, and returns the rendered image inline without hardcoded integrations or manual app-switching.

### Critical Analysis
* **Advantages:** Replaces static API bindings with **bi-directional real-time streaming**, model-agnostic capability discovery, and reusable tool servers.
* **Limitations & Risks:** Lacks native sandboxing. Academic auditing reveals **56% of real-world MCP servers hold unrestricted system access**, exposing hosts to **Tool Poisoning** (found in 5.5% of servers) and indirect prompt injection.
* **Comparison vs. REST:** REST demands predefined static endpoints and client-side code generation; MCP offers **runtime reflection**, allowing models to discover callable schemas on-the-fly.o

### Primary & Academic References
1. **Model Context Protocol Specification**, LF Projects LLC & Anthropic, PBC. `https://modelcontextprotocol.io/specification/latest`
2. Hou, X., et al. (2026). *Model Context Protocol (MCP): Landscape, Security Threats, and Future Research Directions*. ACM TOSEM.
3. Hasan, M. M., et al. (2026). *Model Context Protocol (MCP) at First Glance: Studying Security and Maintainability*. ACM TOSEM.
4. Li, Z., et al. (2025). *We Urgently Need Privilege Management in MCP*. IEEE MASS.

# Illustrator Correction Table — Model Context Protocol (MCP)

| Design Section                          | AI Image Text (Glitch / Corrupted)                                                                                                                                           | Correct Technical English (Copy-Paste to Illustrator)                                                                                                                                                                                                         |                      |
| :-------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------- |
| **Main Header**                         | `MCP (MODEL CONTEXT PROTOCOL)`<br>`PADRÃO ABERTO PARA CONEXÃO DINÂMICA DE MODELOS DE IA`                                                                                     | **`MCP (Model Context Protocol)`**<br>**`TCP/IP APPLICATION LAYER (LAYER 4)`**<br>`Universal Open Standard for Dynamic AI Context & Tool Interoperability`                                                                                                    |                      |
| **Top Diagram (Architecture)**          | • Left box has duplicated dashed `MCP SERVER` at bottom.<br>• Corrupted text: *"JSDH RPC (via cidio/HTTP/ESE)"*.                                                             | **Left Box:** `[ MCP HOST (IDE / LLM) ]`<br>$\updownarrow$<br>`[ MCP CLIENT ]`<br>**Transport Tunnel (bidirectional arrow):**<br>`JSON-RPC 2.0 (via stdio / HTTP-SSE)`<br>**Right Box:** `[ MCP SERVER ]` $\rightarrow$ `[ Local DB / Files / Tools ]`        |                      |
| **Top Diagram (Message Flow)**          | Generic arrows without protocol steps.                                                                                                                                       | **Add 3 numbered protocol steps across the tunnel:**<br>`1. Handshake (initialize)`<br>`2. Reflection (tools/list)`<br>`3. Execution (tools/call)`                                                                                                            |                      |
| **M × N Bottleneck (Central Graphics)** | *"WRAPPERS ESST API ESTATICOS (MANUAL)"*<br>vs.<br>*"MCP - INTEGRAÇÃO DINAMICA E ABTOMATICA"*                                                                                | **Left (Tangled Web):** `Fragmented Static Wrappers (M × N Manual Bindings)`<br>**Right (Central Hub):** `MCP Composable Integration (O(M + N) Open Standard)`                                                                                                |                      |
| **History & Adoption (Calendar Icon)**  | • Calendar year: *"2604"*.<br>• *"25- MILHÕES DE OOWIN DAOSTEXMNNA"*.<br>• *"ABOTADO POR OPENNI, DEMINI, CEGUOELARE, MICROSOFT"*.                                            | **Timeline:** `Origin (Late 2024): Anthropic → Donated to Linux Foundation (LF Projects)`<br>**Scale:** `25M+ Weekly SDK Downloads`<br>**Adoption:** `Adopted by OpenAI, Google Gemini, Cloudflare, Microsoft`                                                |                      |
| **Curiosity & Security (Shield Icon)**  | • *"INSPIRADO NO LSP, MAS COM RISCOS DE SEGURANÇA (CVE-2023-4555B)"*.<br>• *"50% SERAIDIIRES COM ACESSO INDESTOITO. SUSCETIYGL A "ENI9L POITONING" E EXEILTRAÇÃO DE DADOS."* | **Header:** `Curiosity & Security: Inspired by LSP (CVE-2025-49596)`<br>`• Modelled after Microsoft's Language Server Protocol (LSP)`<br>`• In-the-wild exploit: Postmark-MCP stealth BCC email exfiltration`<br>`• Security Audits: 56% unrestricted servers | 5.5% Tool Poisoning` |
| **Real-World Case (Browser Window)**    | • *"CURSOR (IDE) -> MCP SERVER (PLAYERRIGHT)"*.<br>• *"CONSULTE E CAPTURE DADOS VIA SERVIDOR MCP"*.                                                                          | **Header:** `Real-World Case: IDE Automation (Cursor + Playwright)`<br>`Prompt: "Capture website screenshot"`<br>$\rightarrow$ `Cursor queries Playwright MCP Server`<br>$\rightarrow$ `Returns inline rendered screenshot without manual API switching`      |                      |
| **Footer (References)**                 | Missing in the generated image.                                                                                                                                              | **References Box:**<br>`Primary References:`<br>`[1] MCP Spec, Linux Foundation / Anthropic (2024)`<br>`[2] Hou et al., ACM TOSEM (2026)`<br>`[3] Li et al., IEEE MASS (2025)`                                                                                |                      |
