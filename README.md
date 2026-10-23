# MASTER PROMPT: Build GovernedText2SQL

## Role

Act as a Principal AI Architect, Knowledge Graph Engineer, Ontology Specialist, Database Architect, and Senior Python Engineer.

Your task is to design, implement, test, benchmark, document, and deliver a complete, executable, production-inspired GitHub repository named:

**GovernedText2SQL — Evaluating LLM, Semantic-Layer, and Ontology-Driven Virtual Knowledge Graph Approaches to Enterprise Text-to-SQL**

Do not deliver only pseudocode, architecture diagrams, or boilerplate. Create actual executable source code, configuration, test cases, sample data, benchmark scripts, Docker infrastructure, and a working demonstration UI.

Work autonomously through implementation, testing, and debugging. Clearly report anything that could not be verified.

---

# 1. Research Objective

The repository must investigate the hypothesis:

**Can ontology-driven Virtual Knowledge Graphs, implemented with Ontop, reduce semantic errors, improve governance, and increase reliability in enterprise Text-to-SQL systems compared with direct LLM generation and conventional semantic-layer-assisted Text-to-SQL?**

Compare three independent approaches:

### Approach A: Direct LLM Text-to-SQL

Architecture:

User Question → LLM → SQL Generation → SQL Validation → Database → Result

Characteristics:
- LLM receives database schema and user question.
- LLM directly generates SQL.
- No explicit semantic layer or ontology.
- Limited knowledge of enterprise business terminology.
- May generate syntactically valid but semantically incorrect queries.
- Must still enforce common safety and access controls.

### Approach B: Semantic-Layer-Assisted Text-to-SQL

Architecture:

User Question → Semantic Catalog → Relevant Business Metadata → LLM → SQL Generation → Validation → Database → Result

Characteristics:
- Uses structured business metadata.
- Defines metrics, dimensions, entities, synonyms, and approved joins.
- Resolves business terminology before SQL generation.
- Uses a conventional semantic layer, not RDF/OWL or Ontop.
- Supports governed metric definitions and relationships.
- Generates SQL directly against physical database tables.

### Approach C: Ontology + VKG + Ontop Text-to-SQL

Architecture:

User Question → Ontology Entity Resolution → Semantic Query Planning → SPARQL Generation → Ontop → SQL Translation → Database → Result

Characteristics:
- Defines enterprise business concepts using RDF, RDFS, OWL, and optionally SKOS.
- Uses a Virtual Knowledge Graph.
- Uses Ontop for ontology-based data access.
- Uses R2RML or Ontop-native mappings to connect ontology concepts to relational tables.
- Translates SPARQL into SQL using Ontop.
- Avoids full knowledge graph materialization.
- Supports explicit semantic relationships, metric definitions, and governance policies.
- Generates and executes queries through the virtual semantic model.

**Important:** Do not assume Approach C is always superior. Measure the circumstances where each approach succeeds or fails.

---

# 2. Technology Stack

Use the following stack unless a compatibility issue requires a documented alternative.

Backend:
- Python 3.11+
- FastAPI
- Pydantic
- SQLAlchemy
- PostgreSQL
- Pandas
- pytest

AI:
- Configurable LLM provider interface
- OpenAI-compatible API support
- Optional local Ollama support
- Structured JSON outputs where supported
- Deterministic prompts and reproducible configurations

Knowledge Graph:
- RDF
- RDFS
- OWL
- SKOS where useful
- Turtle ontology files
- SPARQL
- Ontop
- R2RML or Ontop-native OBDA mappings
- RDFLib for ontology inspection
- SHACL for relevant ontology and metadata validation

Infrastructure:
- Docker
- Docker Compose
- PostgreSQL container
- Ontop container or supported Ontop CLI/service integration
- Streamlit for demonstration UI
- GitHub Actions for CI
- Prometheus-compatible metrics where practical

Keep all three approaches on the same PostgreSQL dataset.

Use a single configured LLM model for all approaches to ensure a fair comparison.

---

# 3. Enterprise Use Case

Build the demonstration around a realistic banking and marketing analytics platform.

The database should represent customers, accounts, campaigns, customer segments, campaign targeting, transactions, and engagement.

Create at least the following tables:

1. customers
2. accounts
3. transactions
4. campaigns
5. campaign_targeting
6. customer_segments
7. segment_memberships
8. email_engagement
9. products
10. customer_product_holdings
11. branches
12. consent_preferences

Generate at least:
- 10,000 synthetic customers
- 20,000 accounts
- 100,000 transactions
- 500 campaigns
- 50 customer segments
- 100,000 email engagement events

Use deterministic random seeds.

Ensure referential integrity and realistic relationships.

Synthetic data must contain deliberate semantic traps, such as:
- Customer versus account counts
- Gross revenue versus net revenue
- Transaction date versus settlement date
- Active versus closed accounts
- Campaign sent versus campaign delivered
- Customer eligibility versus campaign targeting
- Duplicate customer relationships
- Different definitions of an active customer
- Marketing consent restrictions
- Product ownership versus product eligibility

These traps are essential to demonstrating semantic correctness.

---

# 4. Ontology Design

Create an enterprise banking and marketing ontology.

Include classes such as:

- Customer
- Account
- Transaction
- Campaign
- MarketingSegment
- Product
- Branch
- EngagementEvent
- ConsentPreference

Define object properties:

- ownsAccount
- performedTransaction
- belongsToSegment
- targetedByCampaign
- hasProduct
- managedByBranch
- hasEngagement
- hasConsentPreference

Define data properties:

- customerIdentifier
- transactionAmount
- transactionDate
- settlementDate
- campaignStatus
- engagementType
- consentStatus

Create formal business concepts:

- ActiveCustomer
- EligibleCustomer
- NetRevenue
- CampaignConversionRate
- UniqueEngagedCustomer
- ActiveAccount
- MarketingEligibleCustomer

Explicitly document which concepts are ontology classes, derived predicates, computed measures, or policy constraints.

Do not pretend that OWL alone can calculate arbitrary SQL aggregates or enforce operational authorization.

Use SPARQL aggregates, mapped database views, or explicitly governed query templates where appropriate.

Create:
- ontology.ttl
- business_glossary.yaml
- ontology_validation.py
- ontology_documentation.md

Include ontology consistency and vocabulary validation.

---

# 5. Virtual Knowledge Graph Using Ontop

Implement a functioning Ontop integration.

Requirements:

1. Connect Ontop to PostgreSQL.
2. Create ontology-to-relational mappings.
3. Support virtual RDF access.
4. Execute SPARQL queries against Ontop.
5. Translate SPARQL to SQL through Ontop.
6. Capture the translated SQL when supported by the chosen integration.
7. Execute queries without materializing the entire RDF graph.
8. Compare generated SQL against equivalent baseline SQL.

Provide mappings for:
- Customer to customers
- Account to accounts
- Transaction to transactions
- Campaign to campaigns
- MarketingSegment to customer_segments
- EngagementEvent to email_engagement

Use actual working mapping syntax supported by the installed Ontop version.

Do not fabricate Ontop API endpoints or assume every Ontop distribution exposes SQL translation through the same interface.

If a translation-inspection capability is unavailable, implement a documented supported CLI or diagnostic mechanism and report the limitation.

Include at least 15 verified SPARQL queries.

---

# 6. Implement Three Independent Pipelines

Implement a common interface:

TextToSQLPipeline

Required methods:

- initialize()
- generate(question, context)
- validate(query, user_context)
- execute(query, user_context)
- explain()
- get_metrics()

Implement:

DirectLLMPipeline

SemanticLayerPipeline

OntologyVKGPipeline

Return a common result schema:

- question
- approach
- generated_query
- query_language
- translated_sql, if available
- execution_result
- generation_latency
- execution_latency
- total_latency
- token_usage
- estimated_cost
- validation_status
- semantic_validation_result
- governance_decision
- error_details

For Approach C, preserve SPARQL as the generated query and translated SQL as a separate field.

Do not misrepresent SPARQL generation as direct SQL generation.

---

# 7. Semantic Layer Implementation

Implement a real conventional semantic layer for Approach B.

Define:
- Business entities
- Dimensions
- Measures
- Metrics
- Relationships
- Join paths
- Synonyms
- Filters
- Time dimensions
- Metric aggregation rules

Use YAML or JSON configurations.

Example:

Metric: Active Customers

Definition:
Distinct customers with at least one qualifying active account.

SQL expression:
COUNT(DISTINCT customer_id)

Required filter:
account_status = 'ACTIVE'

Relationship:
customers.customer_id = accounts.customer_id

Ensure the semantic-layer pipeline retrieves relevant definitions and passes them to the LLM.

Do not simply inject the entire database schema and rename it a semantic layer.

---

# 8. Benchmark Dataset

Create at least 150 benchmark questions, divided into categories.

| Category | Minimum Questions |
|---|---:|
| Simple filtering and aggregation | 20 |
| Multi-table joins | 20 |
| Business metric interpretation | 25 |
| Ambiguous terminology | 20 |
| Temporal semantics | 15 |
| Marketing and banking domain logic | 20 |
| Governance and authorization | 15 |
| Complex analytical queries | 15 |

Every benchmark question must include:

- Question ID
- Natural language question
- Difficulty level
- Category
- Expected business interpretation
- Reference SQL or equivalent reference execution
- Expected result or evaluation rule
- Applicable user role
- Required metric definitions
- Relevant semantic relationships
- Expected authorization outcome

Include examples such as:

1. How many active customers do we have?
2. What was net revenue last quarter?
3. Which campaigns generated the highest conversion rate?
4. How many customers opened an email but did not click?
5. Which customers own more than two active products?
6. Which segments contain customers without marketing consent?
7. What is the average transaction value for premium customers?
8. How many unique customers engaged with a campaign?
9. Which branches generated the highest transaction volume?
10. How many eligible customers can receive a credit card campaign?

Add ambiguous and adversarial questions designed to expose semantic mistakes.

Use an executable reference implementation as the evaluation oracle.

---

# 9. Fair Experimental Design

All three approaches must use:

- The same underlying dataset
- The same database snapshot
- The same LLM model and version
- The same temperature where supported
- The same user identity and authorization context
- The same query execution timeout
- The same SQL execution environment
- The same benchmark questions
- The same reference answers

Allow each approach to use its intended metadata, but document the amount and type of information supplied.

Track token usage and metadata retrieval overhead.

Run multiple trials per question with configurable seeds where supported.

Separate:
- Generation failures
- Semantic failures
- Execution failures
- Authorization failures
- Infrastructure failures

Avoid treating a refused unsafe query as a Text-to-SQL failure when refusal is the correct behavior.

Include a controlled ablation experiment comparing the VKG pipeline with and without relevant ontology definitions.

---

# 10. Evaluation Metrics

Measure:

### SQL and query correctness

- Syntax validity rate
- Execution success rate
- Execution-result accuracy
- Semantic correctness rate
- Correct join selection rate
- Correct metric interpretation rate
- Ambiguity handling rate

### Governance

- Unauthorized query rejection rate
- Sensitive field exposure rate
- Policy bypass success rate
- Cross-tenant access violation rate
- Audit coverage

### Performance

- P50 latency
- P95 latency
- P99 latency where sample size is adequate
- LLM token usage
- Estimated inference cost
- Database execution time
- Ontop translation overhead

### Reliability

- Failure rate
- Error classification
- Query reproducibility
- Result consistency
- Recovery behavior

Compute per-category and overall results.

Use confidence intervals and paired comparisons where statistically appropriate.

Do not invent benchmark improvements or hardcode favorable results for the VKG approach.

---

# 11. Governance Architecture

Implement a common governance enforcement layer for all three approaches.

Use:

- Authentication context
- Role-based access control
- Attribute-based policies where useful
- Query allowlisting
- Read-only database access
- Query timeouts
- Row-level security
- Column restrictions
- Audit logging

Define roles:

- Marketing Analyst
- Campaign Manager
- Banking Analyst
- Compliance Officer
- Administrator

Demonstrate:
- Allowed queries
- Denied queries
- Sensitive-data restrictions
- Marketing-consent checks
- Tenant isolation
- Audit trails

For the VKG pipeline, distinguish ontology-level semantic constraints from enforceable security policies.

Enforce security at trusted application and database boundaries, not solely through LLM prompts or ontology annotations.

Never grant broader database privileges to one pipeline than another.

---

# 12. Architecture and Repository Structure

Create this structure, adjusting only when technically justified:

governed-text2sql/
- README.md
- pyproject.toml
- .env.example
- docker-compose.yml
- Makefile
- .github/workflows/
- configs/
- data/
  - schema/
  - seed/
  - benchmarks/
- ontology/
  - banking.ttl
  - mappings/
  - validation/
- semantic_layer/
  - entities.yaml
  - metrics.yaml
  - dimensions.yaml
  - relationships.yaml
- src/
  - api/
  - common/
  - llm/
  - pipelines/
    - direct_llm.py
    - semantic_layer.py
    - ontology_vkg.py
  - ontology/
  - ontop/
  - governance/
  - evaluation/
  - observability/
- ui/
  - streamlit_app.py
- tests/
  - unit/
  - integration/
  - governance/
  - benchmarks/
- scripts/
- reports/
- docs/
  - architecture.md
  - methodology.md
  - ontology.md
  - governance.md
  - benchmark_results.md
  - limitations.md

Provide a Mermaid architecture diagram in the documentation.

---

# 13. Interactive Demonstration UI

Build a Streamlit dashboard.

The UI must allow the user to enter a natural language question and compare the three approaches side by side.

For each approach display:

- Generated SQL or SPARQL
- Ontop-translated SQL where available
- Query result
- Business interpretation
- Semantic definitions used
- Governance decision
- Execution status
- Latency
- Token usage
- Estimated cost
- Errors

Include a benchmark dashboard showing:

- Accuracy comparison
- Semantic error comparison
- Governance comparison
- Latency distribution
- Cost comparison
- Accuracy by question category
- Individual query failure analysis

Use actual execution data.

Never display simulated metrics as real experimental findings.

---

# 14. API Requirements

Implement endpoints:

GET /health

POST /query/direct

POST /query/semantic

POST /query/vkg

POST /query/compare

POST /benchmark/run

GET /benchmark/results

GET /ontology/concepts

GET /ontology/relationships

GET /governance/audit

Use Pydantic request and response models.

Provide OpenAPI documentation and example curl commands.

---

# 15. Automated Tests

Write automated tests for:

- Database schema and seed data
- LLM output parsing
- Semantic-layer retrieval
- Ontology loading
- Ontology-to-table mappings
- Ontop connectivity
- SPARQL execution
- SQL translation
- Query validation
- Governance enforcement
- Benchmark scoring
- API behavior
- Failure recovery

Include negative tests for:
- Hallucinated columns
- Incorrect joins
- Incorrect business metrics
- Unauthorized access
- SQL injection
- SPARQL injection
- Prompt injection
- Missing ontology concepts
- Ontop service failure

The integration test suite must exercise a real Ontop instance, not only mocked responses.

Clearly distinguish unit-test coverage from end-to-end integration verification.

---

# 16. Reproducibility

The repository must work through documented commands such as:

docker compose up --build -d

make seed

make test

make benchmark

make dashboard

Pin dependency versions.

Provide a sample .env.example.

Support a mock LLM provider for infrastructure smoke tests, but require a real configured LLM for claims about Text-to-SQL model accuracy.

Do not embed secrets or API keys.

---

# 17. Expected Research Outputs

Produce:

1. A complete executable repository.
2. Architecture documentation.
3. Enterprise ontology.
4. Working Ontop mappings.
5. Three independent Text-to-SQL implementations.
6. Synthetic banking and marketing dataset.
7. At least 150 benchmark questions.
8. Evaluation and scoring engine.
9. Governance implementation.
10. Interactive comparison dashboard.
11. Automated tests.
12. Benchmark methodology.
13. Generated result files.
14. Limitations and threats-to-validity analysis.
15. A final research-style report.

The report must answer:

- Does VKG improve semantic correctness?
- Does it reduce hallucinated joins?
- Does it improve business metric interpretation?
- Does it reduce governance violations?
- What additional latency does Ontop introduce?
- Does ontology-driven querying increase implementation complexity?
- When is a semantic layer sufficient?
- When is a VKG worth the added engineering investment?
- Which types of questions remain difficult for all three approaches?

Only draw conclusions supported by measured results.

---

# 18. Implementation Instructions

Build the project incrementally:

Phase 1: Architecture, database, and synthetic dataset.

Phase 2: Direct LLM Text-to-SQL pipeline.

Phase 3: Conventional semantic-layer pipeline.

Phase 4: Ontology, Ontop mappings, and VKG pipeline.

Phase 5: Common governance and validation framework.

Phase 6: Benchmark suite and evaluation engine.

Phase 7: Streamlit comparison dashboard.

Phase 8: Docker, CI, testing, and documentation.

Phase 9: End-to-end verification and final research report.

After every phase:
- Implement actual code.
- Run available tests.
- Fix discovered issues.
- Record design decisions.
- Continue to the next phase.

Do not stop after generating the project skeleton.

If a required external service or LLM API is unavailable, implement the remaining components, test with documented local fixtures, and clearly identify the unverified integration.

Do not claim production readiness solely because unit tests pass.

---

# 19. Definition of Done

The repository is complete only when:

- PostgreSQL starts successfully.
- Synthetic data loads successfully.
- Ontop connects to PostgreSQL.
- Ontology mappings are validated.
- At least one real SPARQL query executes through Ontop.
- All three pipelines work against the same dataset.
- The comparison API returns results.
- The dashboard displays real pipeline outputs.
- Governance controls are enforced.
- Benchmark scoring works.
- Automated tests pass in the available environment.
- README instructions are reproducible.
- Known limitations are documented.

The final response must summarize:
- Files created
- Architecture decisions
- Tests executed and results
- Services verified
- Benchmark results, if actually run
- Remaining issues
- Exact commands needed to reproduce the demonstration

**Start building the complete repository now. Do not merely explain how to build it.**
