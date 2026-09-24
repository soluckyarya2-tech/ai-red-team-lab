AI Red Team Lab

A hands-on laboratory for learning, testing, and documenting offensive AI security.

This project is built as a controlled environment for researching vulnerabilities in LLM-powered applications, AI agents, RAG systems, and supporting infrastructure.

🎯 Objectives

The lab is designed around a practical security workflow:

LEARN → LAB → REPRODUCE → ATTACK → DEFEND → DOCUMENT → MOVE ON

The goal is to understand how AI systems can fail from an attacker’s perspective, reproduce those failures in a controlled environment, and document the findings clearly.

🔬 Areas of Research

* Prompt Injection
* Indirect Prompt Injection
* LLM Application Security
* AI Agent Security
* MCP Security
* RAG Security
* Tool & Function Abuse
* Authentication & Authorization
* Web Application Security
* AI Supply-Chain Security
* Model & System Prompt Security
* Data Exfiltration
* Adversarial AI Testing
* MITRE ATLAS Mapping
* OWASP LLM Security

🧰 Lab Stack

Component	Purpose
macOS	Host operating system
OrbStack	Container runtime
Kali Linux	Attacker environment
Open WebUI	LLM-powered target application
Ollama	Local model runtime
Burp Suite	Web/API interception and testing
Garak	LLM vulnerability scanning
PyRIT	AI red-team experimentation
Nmap	Network reconnaissance
SQLMap	SQL injection testing
ffuf	Web fuzzing
WPScan	WordPress security testing

🏗️ Architecture

                         macOS
                           │
             ┌─────────────┴─────────────┐
             │                           │
          OrbStack                    Ollama
             │                           │
       ┌─────┴─────┐              Local LLMs
       │           │
     Kali      Open WebUI
   Attacker       Target
       │           │
       └─────┬─────┘
             │
        Burp Suite
        Interception

Ollama runs directly on the macOS host while the attacker environment and target application run inside OrbStack.

This keeps the lab lightweight while providing a realistic environment for testing LLM-powered applications.

📁 Repository Structure

AI-Red-Team-Lab/
│
├── docker/             # Docker / OrbStack configuration
├── labs/               # Hands-on security exercises
├── reports/            # Security assessments and findings
├── results/            # Test results and generated artifacts
├── garak/              # Garak experiments
├── pyrit/              # PyRIT experiments
├── ollama/             # Ollama-related configuration
├── models/             # Model-related lab resources
└── vulnerable-apps/    # Intentionally vulnerable applications

🧪 Testing Methodology

Each vulnerability or attack scenario is approached systematically:

1. Learn — Understand the vulnerability and relevant security concepts.
2. Lab — Build or configure a controlled test environment.
3. Reproduce — Confirm the vulnerability under known conditions.
4. Attack — Explore realistic exploitation paths.
5. Defend — Test mitigations and security controls.
6. Document — Record evidence, impact, reproduction steps, and remediation.
7. Move On — Map the lesson to broader AI security concepts and continue.

📋 Reporting

Findings are documented with:

* Vulnerability description
* Attack surface
* Reproduction steps
* Request/response evidence
* Impact
* Root cause
* Mitigation
* Verification
* Relevant security framework mappings

Where applicable, findings are mapped to frameworks such as MITRE ATLAS and OWASP LLM security guidance.

⚠️ Scope & Ethics

This laboratory is intended for authorized security research and education.

Testing should only be performed against systems, applications, models, and infrastructure that you own or have explicit permission to assess.

The vulnerable applications and attack scenarios in this repository are intended to provide a controlled environment for security experimentation.

🚧 Project Status

Active development

The lab is continuously evolving as new AI attack techniques, defensive controls, tools, and research areas are explored.

⸻

Focus

Understand the system. Break the assumptions. Prove the impact. Fix the weakness. Document the lesson.