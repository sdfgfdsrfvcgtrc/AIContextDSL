# Unified ContextML Specification v0.1
## Purpose
A unified standard for transferring context to AI tools in development.

## Format
YAML-compatible syntax, .ctml files.

## Structure of the Specification
### Global Project Configuration (project.ctml)
```
project:
  name: string              # Project name (mandatory)
  version: SemVer           # Specification version (mandatory)
  context_strategy:
    max_files: integer      # Maximum number of files in prompt (default: 7)
    priority: [string]      # Priority order: ["critical", "depends_on", ...]

dependencies:
  python: "3.10"            # Programming language versions
  nodejs: "18.17"

libraries:
  -
    name: "pydantic"
    version: "2.6.4"
    stability: "prod"       # Stability levels: prod/test/dev
    update_policy: "patch"  # Update policy: patch/minor/major/none

global:
  encoding: "utf-8"          # File encoding
  line_endings: "unix"      # Line endings: win/unix/mac
```

### Files Configuration (files)
```
files:
  - path: string                # Path to file (mandatory)
    type: string                # Type: python/markdown/config/jupyter (mandatory)
    version: SemVer             # File version
    status: string              # Status: active/legacy/deprecated
    critical: boolean           # Criticality for operation
```
### AI Context Information
```
    ai_hint: string             # Description of purpose (e.g., "JWT authentication")
    tags: [string]              # Categories: ["security", "payment", "legacy"]
```
### Dependencies
```
    depends_on:                 # Hard dependencies
      hard: [string]            # Required file paths (e.g., ["crypto.py"])
      soft: [string]            # Optional references (e.g., ["docs/api.md"])
    circular: false             # Prevent cyclic dependencies
```
### Security
```
    security:
      level: "high"             # Levels: critical/high/medium/low
      standards: ["OWASP ASVS"] # Standards followed
      threat_model:             # Risks & mitigations
        - risk: "Spoofing"
          mitigation: "HMAC-256"
```

### Performance
```
    performance:
      avg_execution_time: "250ms"
      bottlenecks: [string]     # Bottleneck areas (e.g., ["calculate_stats() (CPU-bound)"])
```

### Semantic Hints (Optional)
```
    semantic_hints:
      algorithms: 
        - name: "secure_hash"
          complexity: "O(n^2)"
      tech_debt: 
        - location: "lines 45-60"
          reason: "Outdated AES-128 algorithm"
```
## Advanced Features
### Grouping Context
```
context_groups:
  auth_flow:
    name: "Authentication Workflow"
    files: ["src/auth/controller.py", ...]
    shared_context:             # Shared parameters for group
      security_level: "high"
```
### Templates for Repeated Tasks
```
templates:
  db_model:
    ai_hint: "Database model using SQLAlchemy"
    links: ["docs/db_schema.md"]
```
### Environment Variables Mapping
```
env_mapping:
  database_url: 
    dev: "postgres://localhost:5432/dev"
    prod: "postgres://prod-db:5432/prod"
```
## In-File Annotations
Syntax:
```
# contextml: 
#   ai_hint: "JWT Authentication"
#   tags: ["security", "critical"]
#   depends_on: ["crypto.py"]
# ---
```
### Language	Syntax
```
Python	# contextml:
JavaScript	// contextml:
Markdown	<!-- contextml:
Java	/* contextml:
```
## Validation Rules
* All referenced paths in depends_on must exist.
* The version field must conform to SemVer rules.
* Critical files cannot depend on deprecated modules.
* Cyclic dependencies are forbidden.
