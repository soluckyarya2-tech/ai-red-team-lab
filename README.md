AI Red Team Lab

A self-built, sandboxed environment for adversarial testing of LLM applications and local models.

The project focuses on practical AI security research: reproduce an attack, verify the security boundary it crosses, preserve evidence, and document both confirmed findings and findings that fail re-testing.

Architecture

                         Mac Host
                            │
                            │
                    ┌───────▼────────┐
                    │     Ollama     │
                    │    loopback    │
                    │                │
                    │  qwen3:8b      │
                    │  llama3.2:3b   │
                    │  mistral:7b    │
                    └───────┬────────┘
                            │
                      Lab-controlled
                         connection
                             │
               ┌─────────────▼─────────────┐
               │          OrbStack         │
               │                           │
               │  ┌─────────┐  ┌────────┐  │
               │  │  Kali   │  │ Open   │  │
               │  │ toolbox │  │ WebUI  │  │
               │  │         │  │ 0.3.16 │  │
               │  └─────────┘  └────────┘  │
               └───────────────────────────┘

Ollama runs on the Mac host while the offensive tooling and target application run inside the controlled OrbStack environment. The lab is intentionally operated as a switchable research environment rather than a permanently running service.

The design keeps the AI security work separated from normal personal computing: services are started deliberately, unnecessary listeners are not left running, and testing is performed against local instances and synthetic test data.

Methodology

The testing workflow is:

Validate attacker identity
          ↓
       Baseline
          ↓
        Attack
          ↓
        Control
          ↓
       Re-test
          ↓
      Document

1. Validate the attacker token

Authentication and authorization assumptions are checked before drawing a security conclusion.

This became an important research rule after an initial cross-user chat finding was later shown to be a false positive. The finding was re-tested, the original conclusion was withdrawn, and the corrected result was documented.

2. Establish a baseline

Normal application behavior is recorded before introducing an attack.

3. Attack

The attacker-controlled request, payload, document, model interaction, or application state is introduced.

4. Control

The same security boundary is tested with an appropriate control case to distinguish an actual vulnerability from expected behavior.

5. Re-test

A result is not treated as confirmed solely because one request produced an unexpected response. Authentication state, ownership, model behavior, and reproduction conditions are checked again where necessary.

6. Preserve evidence

The repository keeps the material needed to reproduce the research:

* findings/ — findings and their status
* findings/payloads/ — attack payloads
* findings/screenshots/ — visual evidence
* findings/evidence/ — reproduction evidence
* findings/recon/ — reconnaissance material
* findings/superseded/ — material replaced by newer research

The project deliberately preserves withdrawn findings rather than silently deleting them.

Findings Summary

ID	Status	Finding
F1	FIXED	Tested file-content access correctly enforced cross-user ownership.
F2	FIXED	Initial cross-user chat-leak claim was retracted and re-tested; the tested request returned 401.
F3	CONFIRMED	JWTs were issued without an exp claim, allowing tokens to remain valid without an embedded expiration time.
F4	WITHDRAWN	Initial RAG citation-exfiltration claim did not survive re-testing and was explicitly withdrawn.
F5	CONFIRMED	Cross-user denial behavior produced a distinguishable application response.
F6	HIGH	An authenticated user could process another user’s RAG document by supplying its file identifier without an ownership check.
F7a	HIGH	An authenticated user could query RAG collections by name without collection-level ownership verification.
F7b	CONFIRMED	Attacker-controlled content retrieved through RAG could introduce instructions that influenced the model’s response.
M1	CONFIRMED	Fabricated conversation history influenced model behavior under the tested conditions.
M2	CONFIRMED	The fabricated-history technique transferred across the tested model conditions.

RAG Findings

F6, F7a, and F7b test different security boundaries in the RAG pipeline:

F6
Authenticated cross-user document ingestion
                │
                ▼
F7a
Cross-user collection querying
                │
                ▼
F7b
Untrusted retrieved content influencing model behavior

These are documented as separate findings rather than one combined vulnerability because each represents a distinct control failure.

F7b also demonstrated an important limitation: an indirect prompt injection only has an opportunity to influence the model when the malicious instruction is actually present in the retrieved context. The testing therefore distinguishes the attack payload from the retrieval behavior instead of assuming that every stored instruction reaches the model.

Tooling

The lab uses a focused offensive AI security stack:

* Garak 0.17.0 — pinned LLM security testing framework
* Burp Suite — HTTP interception and request analysis
* curl — direct API and authorization testing
* Docker / OrbStack — isolated lab environment
* Kali Linux — security testing toolbox
* Ollama — local model serving
* Open WebUI 0.3.16 — target LLM application

The currently tested local models include:

* qwen3:8b
* llama3.2:3b
* mistral:7b

Lab operation is deliberately explicit:

lab-start
    │
    ├── Start required lab services
    ├── Verify expected components
    └── Run canary checks
lab-down
    │
    └── Stop the lab environment

This keeps the research environment reproducible and prevents unnecessary services from remaining active between testing sessions.

Scope & Ethics

This repository documents security research performed against systems owned and controlled for testing.

* Testing is limited to the local AI Red Team Lab.
* Test accounts and documents are synthetic.
* No third-party systems are targeted.
* No real user data is required for the documented findings.
* Security claims are re-tested before being treated as confirmed.
* False positives are explicitly marked as withdrawn.
* Evidence is preserved so that conclusions can be independently reviewed.

The objective is to develop practical AI security testing methodology while maintaining a clear boundary between controlled research and unauthorized testing.

Lessons From the Research

Two lessons from this project now form part of the testing methodology.

Token validation comes before the verdict

A request that appears to cross an authorization boundary is not sufficient evidence until the attacker identity and token are independently validated.

Authentication mistakes can create convincing but incorrect vulnerability reports.

Incomplete output is not the same as refusal

LLM security testing cannot rely only on whether the model produced the exact expected attack output.

A model may partially comply, transform an instruction, reveal part of a payload, or follow an injected instruction without reproducing the complete requested text.

For this reason, model behavior is evaluated from the security effect of the interaction, not only from exact string matching.

Roadmap

The next research stages are:

1. Expand indirect prompt-injection variants and retrieval-position testing.
2. Add PyRIT-based attack amplification.
3. Run a broader Garak baseline against the local models.
4. Investigate the remaining application/API leads.
5. Continue the queued RAG and authorization probes.
6. Prepare responsible disclosure material if an applicable external target is identified.

The immediate objective is not to maximize the number of findings. It is to produce a smaller set of findings that are reproducible, evidence-backed, and technically defensible.