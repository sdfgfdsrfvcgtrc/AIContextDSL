# ContextML Specification v0.1

## Context Binding Language for AI Systems

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

### Language	Syntax
```
* Python: # contextml:
* JavaScript: // contextml:
* Markdown: <!-- contextml: -->
* Java: /* contextml: */
* TypeScript: // contextml:
* YAML: # contextml:
* JSON: // contextml:
* TOML: # contextml:
* XML: <!-- contextml: -->
* HTML/CSS: <!-- contextml: -->
* Jupyter Notebooks: # contextml:
* Shell scripts: # contextml:
* SQL queries: -- contextml:
* Dockerfiles: # contextml:
* Text documents: # contextml:
* GraphQL schemas: # contextml:
* Protocol buffers: // contextml:
* R scripts: # contextml:
* Package lock files: // contextml:
* C++ source code: // contextml:
* Rust programs: // contextml:
* Go applications: // contextml:
* Kotlin classes: // contextml:
* Swift projects: // contextml:
* Binary data: # contextml:
* Comma-separated values: # contextml:
* INI configurations: ; contextml:
* General configuration files: # contextml:
```
## Validation Rules
* All referenced paths in depends_on must exist.
* The version field must conform to SemVer rules.
* Critical files cannot depend on deprecated modules.
* Cyclic dependencies are forbidden.

## Cycle Dependency Check
The system is required to monitor the presence of cyclic dependencies between files and prevent their creation. If a cyclic dependency is detected, an error message should be displayed.

## Non-Critical Module Control
If a critical file depends on an outdated component (module), the system must issue a warning or error indicating the need to update dependencies. It is recommended to regularly check the version of dependencies and promptly update components to the current version.

## Support for International Encoding Standards
The project must support different international file encoding standards such as UTF-8 and UTF-16. Developers can choose the appropriate encoding standard for their specific project by specifying it in the configuration file (project.ctml).

The recommended default encoding format is UTF-8 because it is widely used and compatible with most modern platforms and services.

```
## In-File Annotations
An important feature of ContextML is its ability to add internal annotations directly inside individual files. These annotations provide additional metadata that helps both developers and automated systems understand the purpose, structure, and relationships within the project.
Annotations come in two forms: manual and automatic.

### Manual Annotations
Manual annotations are created explicitly by developers themselves. They represent deliberate decisions about the code or content and serve several purposes:
* Provide clear instructions for AI interpreters.
* Highlight key functionalities or risks associated with certain sections of the code.
* Tag files or parts of them into categories for easier filtering.

Example:
```
# contextml-manual:
#   id: "auth-flow-annotation"
#   hint: "JWT Authentication flow implementation"
#   tags: ["security", "authentication"]
```
Each annotation includes:
* ID: Unique identifier.
* Hint: Human-readable description of the annotation’s intent.
* Tags: List of categorization labels applied to the annotated section.

### Automatic Annotations
Automatic annotations are generated programmatically by parsers or other tools based on analysis of the file contents. They help supplement information automatically without requiring explicit developer intervention but may not always reflect accurate user intentions fully.

Examples might include:
* Detection of potential performance issues.
* Identification of unused imports or obsolete functionality.

They follow the same basic structure as manual annotations but typically have additional fields like source, which indicates where they originated from (e.g., a linter or analyzer tool).

Example:
```
# contextml-automatic:
#   id: "perf-bottleneck-123"
#   hint: "Potential slowdown due to nested loops."
#   source: "PerformanceAnalyzer-v2.1"
```
### Difference Between Manual and Automatic Annotations

* Priority: Manual annotations always override automatic ones when conflicts arise.
* Editable: Only manual annotations can be edited freely by developers. Automated annotations will only change through re-analysis unless specifically modified manually.
* Usage Scenarios: Use manual annotations for critical contexts or essential points you want highlighted. Use automatic annotations for general insights derived by tools during static analysis.

### Example Combining Both Annotation Types
Here’s an example combining both kinds of annotations in one file, where automatic annotations appear first, followed by manual annotations:

Example:
```
# contextml-automatic:
#   id: "unused-import-warning"
#   hint: "Unused import 'os' found."
#   source: "LinterTool-v1.5"

# contextml-manual:
#   id: "auth-flow-annotation"
#   hint: "JWT Authentication flow implementation"
#   tags: ["security", "authentication"]
```
### Recommendation
Developers should primarily rely on manual annotations for high-level guidance, whereas automatic annotations complement these efforts by providing deeper insights into less obvious patterns.


