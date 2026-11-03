# Wolio — Implementation Specification v1

**Status:** Draft for Implementation  
**Specification Version:** 1.0  
**Target:** First production-oriented implementation baseline  
**Language:** Wolio  
**Compiler Implementation:** Rust  
**Native Backend:** LLVM  
**Primary Goal:** AI-native software construction without AI/IDE/model lock-in

---

# 1. Purpose

Dokumen ini mendefinisikan **Implementation Specification v1** untuk Wolio.

Dokumen ini berada satu tingkat di bawah `Wolio Blueprint`.

Blueprint mendefinisikan:

- mengapa Wolio dibuat;
- masalah yang ingin diselesaikan;
- seluruh arah arsitektur;
- seluruh fitur yang direncanakan;
- prinsip desain;
- batasan fundamental;
- roadmap jangka panjang.

Implementation Specification mendefinisikan:

- komponen yang benar-benar dibangun pada baseline v1;
- struktur repository;
- crate/module;
- API internal;
- data structures;
- protocol;
- invariants;
- error model;
- verification pipeline;
- CLI;
- compiler pipeline;
- acceptance criteria.

Blueprint tetap menjadi **helicopter view** dan tidak boleh dipersempit hanya karena implementasi saat ini belum mencakup seluruh fitur.

---

# 2. Specification Versioning

Wolio menggunakan dua konsep versioning yang berbeda.

## 2.1 Blueprint Version

Blueprint menggambarkan **arah keseluruhan Wolio**.

Contoh:

```text
Wolio Blueprint v1
Wolio Blueprint v2
Wolio Blueprint v3
```

Blueprint version berubah ketika visi atau arsitektur besar Wolio berubah.

---

## 2.2 Implementation Specification Version

Implementation Specification menggambarkan **baseline implementasi**.

Contoh:

```text
Implementation Specification v1
Implementation Specification v1.1
Implementation Specification v1.2
Implementation Specification v2
```

### v1.x

Digunakan untuk:

- bug fix;
- clarification;
- implementation refinement;
- diagnostic improvement;
- performance improvement;
- backward-compatible additions.

### v2

Digunakan apabila terjadi perubahan fundamental terhadap:

- language semantics;
- compiler architecture;
- semantic model;
- protocol;
- ABI;
- package format;
- compatibility model;
- project structure.

---

# 3. Relationship Between Specifications

Struktur resmi:

```text
                    WOLIO BLUEPRINT
                         │
                         │ defines WHAT / WHY
                         ▼
             IMPLEMENTATION SPECIFICATION
                         │
                         │ defines HOW
                         ▼
                   IMPLEMENTATION
                         │
                         ▼
                  TEST / VERIFICATION
                         │
                         ▼
                   RELEASE BASELINE
```

AI coding agent **tidak boleh menggunakan Blueprint sebagai satu-satunya implementation instruction**.

AI harus menggunakan:

```text
Blueprint
+
Implementation Specification
+
Task Specification
```

sebagai sumber konteks.

---

# 4. Design Philosophy

Wolio bukan sekadar:

> "programming language with stricter syntax."

Wolio adalah:

> **AI-native software construction system with a programming language, semantic model, compiler, verification engine, and machine-readable development protocol.**

Problem utama yang ingin diselesaikan:

```text
AI
 ↓
writes code extremely fast
 ↓
code compiles
 ↓
algorithm may still be wrong
 ↓
AI creates workaround
 ↓
technical debt increases
 ↓
future AI loses project consistency
```

Wolio mengubah workflow menjadi:

```text
INTENT
  ↓
SPECIFICATION
  ↓
CONTRACT
  ↓
PLAN
  ↓
SCOPED CHANGE
  ↓
SEMANTIC MUTATION
  ↓
ANALYSIS
  ↓
VERIFICATION
  ↓
BUILD
```

Strictness bukan tujuan akhir.

**Verifiability adalah tujuan.**

---

# 5. Core Principles

Wolio v1 wajib mempertahankan prinsip berikut.

## 5.1 Static Typing

Semua value memiliki type yang dapat diverifikasi compiler.

---

## 5.2 Semantic Types

Type tidak hanya merepresentasikan representation.

Contoh:

```wol
type ProductPrice = Money<IDR>
type AccountBalance = Money<IDR>
type TaxAmount = Money<IDR>
```

Ketiganya dapat menggunakan representation yang sama tetapi memiliki semantic identity berbeda.

AI tidak boleh menganggap semuanya interchangeable hanya karena underlying representation sama.

---

## 5.3 Result / Option

Error dan absence harus eksplisit.

```wol
Result<User, UserNotFound>
```

dan:

```wol
Option<User>
```

Tidak boleh ada implicit null/error propagation.

---

## 5.4 Exhaustive Error Handling

Semua error yang diwajibkan contract harus ditangani secara eksplisit.

---

## 5.5 Explicit Dependencies

Dependency harus dapat ditelusuri.

---

## 5.6 Explicit Effects

Operation harus mendeklarasikan effect yang dapat dilakukan.

Contoh:

```wol
operation CreateUser {
    effects:
        Database.Write
}
```

Operation dengan:

```wol
effects: none
```

tidak boleh melakukan database write secara diam-diam. Explicit effect merupakan salah satu invariant utama desain Wolio.

---

## 5.7 Contracts

Contract mendefinisikan semantic boundary:

```text
INPUT
PRECONDITION
EFFECT
OUTPUT
POSTCONDITION
ERRORS
INVARIANTS
DEPENDENCIES
```

---

## 5.8 Semantic Identity

Semantic entities memiliki identity yang tidak bergantung pada line number.

Filesystem path juga bukan identity.

Hal ini secara eksplisit memperbaiki desain lama yang terlalu menjadikan filesystem sebagai semantic type system. Kritik desain sebelumnya menyarankan filesystem semantics dan line-number identity dibuang.

---

## 5.9 Change Scope

Setiap machine-generated change harus memiliki scope.

---

## 5.10 Impact Analysis

Sebelum perubahan diterapkan, Wolio harus dapat mengetahui:

```text
affected operations
affected contracts
affected dependencies
affected tests
affected modules
affected invariants
```

---

# 6. V1 Feature Boundary

Implementation Specification v1 membagi fitur menjadi tiga kategori.

## 6.1 V1 Mandatory

```text
Static typing
Result / Option
Exhaustive matching
Typed errors
Semantic types
Semantic IDs
Contracts
Explicit dependencies
Explicit effects
Semantic graph
Change scope
Impact analysis
Verification
Diagnostics
CLI
Package manifest
Lockfile
Wolio IR
LLVM backend
Native executable
Wolio Tool Protocol
Basic LSP integration
```

---

## 6.2 V1 Foundation / Limited

Fitur berikut harus memiliki foundation pada V1 tetapi tidak perlu seluruh advanced semantics:

```text
Concurrency
Security
Property verification
Architecture contracts
Repository history
Package registry
IDE integration
```

---

## 6.3 Post-V1

```text
Advanced ownership
Formal verification
Advanced generics
Advanced concurrency semantics
Distributed semantic verification
Autonomous repair
Advanced proof system
Advanced security contracts
Full IDE ecosystem
```

Ini mempertahankan keputusan sebelumnya agar fitur seperti ownership, formal verification, dan complex concurrency tidak dipaksakan menjadi V1 hanya karena menarik secara teoritis.

---

# 7. High-Level Architecture

```text
                         USER / AI
                            │
              ┌─────────────┴─────────────┐
              │                           │
             IDE                    AI Coding Agent
              │                           │
              └─────────────┬─────────────┘
                            │
                    Wolio Tool Protocol
                            │
                            ▼
                     Wolio CLI / Server
                            │
             ┌──────────────┼──────────────┐
             │              │              │
             ▼              ▼              ▼
         Semantic       Compiler       Verification
          Engine         Engine           Engine
             │              │              │
             └──────────────┼──────────────┘
                            │
                            ▼
                     Wolio Semantic IR
                            │
                            ▼
                        Wolio IR
                            │
                            ▼
                       LLVM IR
                            │
                            ▼
                       Native Code
```

---

# 8. AI / IDE Independence

## 8.1 Fundamental Rule

Wolio **MUST NOT require a specific AI provider**.

Tidak boleh ada dependency:

```text
Wolio → OpenAI
Wolio → Anthropic
Wolio → Google
```

Sebaliknya:

```text
AI Provider
     ↓
Tool Protocol
     ↓
Wolio
```

---

# 9. Wolio Tool Protocol

Nama sementara:

```text
WTP
Wolio Tool Protocol
```

WTP adalah protocol untuk mengakses semantic/compiler capabilities Wolio.

WTP **bukan AI API**.

WTP tidak:

- menyediakan LLM;
- memilih model;
- menyimpan API key AI;
- mengontrol IDE;
- mengharuskan autonomous agent;
- menentukan provider AI.

WTP hanya menyediakan kemampuan Wolio kepada external tools.

---

# 10. WTP Consumers

Consumer dapat berupa:

```text
VS Code extension
JetBrains plugin
Zed extension
Neovim plugin
Claude Code adapter
Codex adapter
Gemini adapter
Cursor adapter
Custom AI agent
Internal company tooling
Human CLI usage
CI/CD system
```

Semua consumer harus mendapatkan semantic capabilities yang sama.

---

# 11. WTP Capabilities

Contoh capability:

```text
project.inspect
module.inspect
operation.inspect
contract.inspect
symbol.inspect

dependency.inspect
effect.inspect
graph.query
impact.analyze

change.plan
change.validate
change.apply
change.rollback

test.inspect
verification.run
diagnostic.inspect
```

---

# 12. Source Editing

Wolio tidak mewajibkan AI menggunakan structured mutation.

Ada dua mode.

## Mode A — Source Editing

AI/IDE dapat mengubah:

```text
*.wol
```

secara langsung.

Kemudian:

```text
parse
→ semantic analysis
→ verification
```

menentukan apakah perubahan valid.

---

## Mode B — Semantic Mutation

AI dapat meminta:

```text
create_operation
modify_contract
add_test
rename_symbol
add_dependency
remove_dependency
modify_effect
```

Compiler kemudian melakukan mutation terhadap semantic representation.

---

## 12.1 Tidak Ada Vendor Lock-in

Mode B adalah **capability**, bukan kewajiban.

Dengan demikian:

```text
AI A → source editing
AI B → WTP mutation
AI C → hybrid
Human → normal editor
```

semuanya valid.

---

# 13. Why WTP Exists

Tujuannya bukan membuat AI bergantung pada Wolio.

Tujuannya adalah memberikan **jalan yang lebih aman** bagi AI yang ingin memahami project secara semantic.

Tanpa WTP:

```text
AI
 ↓
grep
 ↓
read source
 ↓
guess
 ↓
edit file
 ↓
compile
```

Dengan WTP:

```text
AI
 ↓
inspect semantic entity
 ↓
inspect contract
 ↓
inspect dependencies
 ↓
inspect impact
 ↓
propose change
 ↓
verify
```

Semantic AST harus menjadi first-class citizen dan AI tidak seharusnya dipaksa scraping source code.

---

# 14. Compiler Architecture

Compiler implementation menggunakan Rust.

Pipeline:

```text
Source
  ↓
Lexer
  ↓
Parser
  ↓
AST
  ↓
Name Resolution
  ↓
Type Checking
  ↓
Semantic Analysis
  ↓
Contract Analysis
  ↓
Effect Analysis
  ↓
Dependency Analysis
  ↓
Verification
  ↓
Wolio IR
  ↓
LLVM IR
  ↓
LLVM
  ↓
Native Binary
```

LLVM digunakan sebagai backend, bukan custom CPU backend.

---

# 15. Cargo Workspace

Repository compiler:

```text
wolio/
├── Cargo.toml
│
├── crates/
│   ├── wolio-lexer/
│   ├── wolio-parser/
│   ├── wolio-ast/
│   ├── wolio-types/
│   ├── wolio-semantic/
│   ├── wolio-contracts/
│   ├── wolio-effects/
│   ├── wolio-resolver/
│   ├── wolio-analyzer/
│   ├── wolio-verify/
│   ├── wolio-ir/
│   ├── wolio-codegen/
│   ├── wolio-runtime/
│   ├── wolio-package/
│   ├── wolio-agent/
│   ├── wolio-cli/
│   ├── wolio-format/
│   └── wolio-lsp/
│
├── tests/
├── fixtures/
├── benchmarks/
└── docs/
```

---

# 16. Crate Responsibility

## `wolio-lexer`

Responsibilities:

```text
source → tokens
```

Must not:

```text
resolve symbols
type check
access filesystem
invoke LLVM
```

---

## `wolio-parser`

Responsibilities:

```text
tokens → AST
```

---

## `wolio-ast`

Source representation.

AST harus immutable setelah parsing.

---

## `wolio-types`

Menyediakan:

```text
PrimitiveType
SemanticType
GenericType
ResultType
OptionType
FunctionType
OperationType
```

---

## `wolio-semantic`

Central semantic model.

```text
SemanticRepository
SymbolIndex
OperationIndex
RelationGraph
SemanticId
```

---

## `wolio-contracts`

Contract representation dan verification.

---

## `wolio-effects`

Effect declarations dan checking.

---

## `wolio-resolver`

Name/module/dependency resolution.

---

## `wolio-analyzer`

Non-fatal semantic analysis.

Analyzer dapat menghasilkan:

```text
warning
risk
suspicion
recommendation
```

Tidak semua analyzer finding harus menjadi compiler error.

---

## `wolio-verify`

Verification pipeline.

```text
type
contract
effect
dependency
test
property
invariant
```

---

## `wolio-ir`

Compiler intermediate representation.

---

## `wolio-codegen`

Wolio IR → LLVM IR.

---

## `wolio-runtime`

Runtime primitives.

---

## `wolio-package`

Package manager, registry interaction, lockfile.

---

## `wolio-agent`

WTP implementation.

Nama crate ini boleh berubah menjadi:

```text
wolio-protocol
```

untuk menghindari kesan bahwa Wolio memiliki AI agent sendiri.

---

## `wolio-cli`

Single binary:

```bash
wolio init
wolio check
wolio build
wolio run
wolio test
wolio verify
wolio inspect
wolio impact
wolio graph
wolio explain
wolio add
wolio remove
wolio install
wolio update
```

---

## `wolio-lsp`

Language Server Protocol.

---

# 17. Semantic Repository

Semantic repository merupakan source of truth selama compilation/analysis.

Minimal:

```text
SemanticRepository
├── SymbolIndex
├── OperationIndex
├── ContractIndex
├── TypeIndex
├── DependencyGraph
├── EffectGraph
├── RelationGraph
└── TestIndex
```

Contoh API:

```rust
pub trait SemanticRepository {
    fn find_symbol(
        &self,
        id: SemanticId
    ) -> Option<&Symbol>;

    fn find_operation(
        &self,
        id: SemanticId
    ) -> Option<&Operation>;

    fn dependencies(
        &self,
        id: SemanticId
    ) -> &[SemanticId];

    fn callers(
        &self,
        id: SemanticId
    ) -> &[SemanticId];
}
```

Invariant:

```text
O(1) lookup where indexed
deterministic
read-only queries
no parser invocation
no LLVM invocation
no hidden mutation
```

Pendekatan granular seperti ini memang diperlukan sebelum coding dimulai.

---

# 18. Semantic ID

Semantic identity tidak boleh menggunakan:

```text
line number
file offset
filesystem path
```

Semantic ID harus tetap stabil ketika source dipindahkan atau diformat ulang.

Contoh konseptual:

```text
operation.payment.create
entity.payment.Payment
contract.payment.create
```

Implementasi internal dapat menggunakan stable identifier/hash.

---

# 19. Contracts

Contract minimum:

```text
INPUT
REQUIRES
EFFECTS
OUTPUT
ENSURES
ERRORS
INVARIANTS
DEPENDENCIES
```

Contoh:

```wol
operation CreatePayment {

    input:
        request: PaymentRequest

    requires:
        request.amount > 0

    dependencies:
        PaymentRepository
        IdempotencyStore

    effects:
        Database.Read
        Database.Write

    output:
        Payment

    errors:
        InvalidAmount
        DuplicateRequest
        PersistenceFailure
}
```

---

# 20. Contract Is Versioned

Contract bukan immutable eternal truth.

Contract adalah:

```text
versioned decision
```

Jika requirement berubah:

```text
Contract v3
    ↓
Contract Change Proposal
    ↓
Impact Analysis
    ↓
Approval
    ↓
Contract v4
```

AI tidak boleh mengubah contract secara diam-diam.

---

# 21. Semantic Operations

Function tetap ada sebagai implementation mechanism.

Tetapi semantic abstraction utama adalah:

```text
Operation
```

Contoh:

```wol
operation CreatePayment {

    consumes:
        PaymentRequest

    produces:
        PaymentCreated

    requires:
        CustomerActive
        AmountPositive
        IdempotencyValid

    effects:
        Database.Write

}
```

Implementation:

```wol
implement CreatePayment {
    ...
}
```

---

# 22. Semantic Relationships

Wolio harus menyimpan hubungan:

```text
consumes
produces
requires
guarantees
depends_on
affects
invalidates
implements
tests
calls
```

Jadi semantic graph bukan sekadar dependency graph.

Contoh:

```text
ProductUnitPrice
        │
        ▼
CalculateOrderSubtotal
        │
        ▼
OrderSubtotal
        │
        ▼
Order
        │
        ▼
PaymentIntent
```

Ini diperlukan karena type checking saja tidak cukup untuk menangkap business-semantic mismatch.

---

# 23. Verification Layers

Verification pipeline:

```text
                    CHANGE
                      │
                      ▼
                Scope Validation
                      │
                      ▼
                 Parse Check
                      │
                      ▼
                 Type Check
                      │
                      ▼
              Semantic Verification
                      │
          ┌───────────┼───────────┐
          ▼           ▼           ▼
       Contract     Effect      Dependency
          │           │           │
          └───────────┼───────────┘
                      ▼
                Impact Analysis
                      │
                      ▼
                    Tests
                      │
                      ▼
                  Invariants
                      │
                      ▼
                 Build / LLVM
```

Arsitektur verification ini mempertahankan arah sebelumnya: impact analysis → type/test/property → invariant → compiler.

---

# 24. Analyzer vs Compiler

Wolio membedakan:

## Compiler

Menentukan:

```text
LEGAL
vs
ILLEGAL
```

Contoh:

```text
type mismatch
undeclared symbol
invalid effect
missing dependency
invalid syntax
```

---

## Analyzer

Mendeteksi:

```text
POSSIBLY WRONG
RISK
SUSPICIOUS
```

Contoh:

```text
unusual fallback
suspicious constant
unexpected dependency
semantic substitution
high complexity
possible race
```

Dengan demikian Wolio tidak menjadi bahasa yang terlalu dogmatis.

---

# 25. Anti-Workaround

Wolio harus mampu mendeteksi pola:

```text
fallback substitution
effect substitution
capability bypass
boundary bypass
hardcoded workaround
unauthorized persistence
```

Contoh:

Contract:

```text
intent:
    PersistUser

effect:
    Database.Write
```

AI mengganti:

```text
Database.Write
```

dengan:

```text
HTTP POST localhost
```

Tidak otomatis dianggap valid.

Semantic analyzer harus mampu mendeteksi:

```text
declared intent:
PersistUser

implementation:
HTTP.Request

semantic mismatch
```

---

# 26. Fallback Policy

Fallback bukan default.

Wolio membedakan:

```text
DEFAULT
FALLBACK
RECOVERY
SUBSTITUTION
MOCK
PLACEHOLDER
```

Contoh valid:

```wol
default timeout = 30s
```

Tetapi:

```text
UserNotFound
    ↓
DefaultUser
```

harus ditolak apabila contract tidak mengizinkannya.

---

# 27. Secrets

Secret tidak boleh disimpan sebagai ordinary source constant.

Contoh berikut harus ditolak:

```wol
api_key = "secret"
```

Secret harus berasal dari explicit secret capability/configuration mechanism.

Compiler/analyzer harus mendeteksi:

```text
API keys
passwords
tokens
private keys
credential-like constants
```

---

# 28. Change Scope

Setiap machine-generated modification harus memiliki:

```text
scope
actor
reason
target
expected impact
```

Contoh:

```text
ChangeScope {
    target: operation.payment.create
    reason: "add idempotency validation"
    allowed:
        operation.payment.create
        contract.payment.create
        tests.payment.create
}
```

AI tidak boleh melakukan perubahan di luar scope tanpa proposal baru.

---

# 29. Impact Analysis

Sebelum mutation:

```text
impact(target)
```

menghasilkan:

```text
affected symbols
affected operations
affected contracts
affected tests
affected modules
affected dependencies
affected invariants
```

Contoh:

```text
Change:
PaymentRequest.amount

Impact:
17 operations
42 tests
3 modules
2 contracts
1 invariant
```

---

# 30. Test Model

V1 tidak memaksakan:

```text
exactly 2 tests/function
```

sebagai universal language rule.

Namun setiap semantic operation harus memiliki test coverage yang dapat diverifikasi berdasarkan contract dan project policy.

Minimal policy dapat menetapkan:

```text
success case
failure/boundary case
```

tetapi jumlah test adalah project policy, bukan fundamental syntax rule.

Ini memperbaiki keputusan lama yang terlalu rigid mengenai universal two-tests-per-function.

---

# 31. Filesystem

Filesystem digunakan untuk organization.

Filesystem **BUKAN type system**.

Path:

```text
src/payment/create.wol
```

tidak menentukan semantic identity.

Refactoring:

```text
payment/create
→ billing/payment/create
```

tidak boleh secara otomatis mengubah semantic identity.

Tooling dapat mengetahui relocation sebagai repository change, tetapi identity tetap stabil.

---

# 32. Project Layout

Contoh:

```text
hello/
├── wolio.toml
├── wolio.lock
├── src/
│   ├── main.wol
│   └── hello.wol
├── tests/
│   └── hello.test.wol
└── .wolio/
    ├── ast/
    ├── graph/
    ├── symbols/
    ├── contracts/
    ├── tests/
    └── history/
```

`.wolio/` adalah machine-readable semantic cache, bukan source of truth.

Source tetap dapat direconstruct/verified dari project source.

---

# 33. Package Management

Single binary:

```bash
wolio add
wolio remove
wolio install
wolio update
```

Lockfile:

```text
wolio.lock
```

harus menyimpan sekurang-kurangnya:

```text
package
version
source_hash
dependency_hash
compiler_version
contract_hash
```

Tujuannya memastikan AI tidak bekerja terhadap dependency atau compiler state yang tidak diketahui.

---

# 34. Compiler Version

Project harus menyatakan language/compiler compatibility.

Contoh:

```toml
[language]
version = "1.0"
```

Compiler incompatible tidak boleh silently compile project.

Migration harus eksplisit.

---

# 35. CLI

V1 harus menyediakan:

```bash
wolio init
wolio check
wolio build
wolio run
wolio test
wolio verify

wolio inspect
wolio impact
wolio graph
wolio explain

wolio add
wolio remove
wolio install
wolio update

wolio fmt
wolio lint
```

---

# 36. Diagnostics

Semua diagnostic harus machine-readable.

Contoh:

```json
{
  "code": "WOLIO-EFFECT-001",
  "severity": "error",
  "category": "effect",
  "message": "Operation declared effects:none but performs Database.Write",
  "target": "operation.payment.create",
  "location": {
    "file": "src/payment/create.wol",
    "line": 42
  },
  "repair": {
    "allowed": ["declare Database.Write", "remove Database.Write"]
  }
}
```

Diagnostic harus dapat dikonsumsi:

```text
human
IDE
CI
AI agent
```

---

# 37. AI Repair Protocol

Compiler error harus dapat menjadi structured repair information.

Contoh:

```text
ERROR
 ↓
CAUSE
 ↓
AFFECTED ENTITY
 ↓
CONSTRAINT
 ↓
ALLOWED REPAIRS
 ↓
FORBIDDEN REPAIRS
```

AI tidak perlu menebak dari error text.

---

# 38. WTP Inspect Example

Request:

```json
{
  "operation": "operation.inspect",
  "id": "payment.create"
}
```

Response:

```json
{
  "id": "payment.create",
  "contract": "payment.create.v3",
  "inputs": [],
  "outputs": [],
  "effects": [],
  "dependencies": [],
  "invariants": [],
  "tests": [],
  "callers": [],
  "callees": []
}
```

---

# 39. WTP Change Example

AI dapat meminta:

```json
{
  "operation": "change.plan",
  "target": "payment.create",
  "intent": "add idempotency validation"
}
```

Wolio mengembalikan:

```text
proposed changes
affected contracts
affected tests
affected dependencies
risk
required approval
```

Baru kemudian:

```text
change.apply
```

---

# 40. AI Model Freedom

Wolio tidak mengetahui:

```text
which model generated the change
```

Wolio hanya mengetahui:

```text
actor
scope
mutation
verification result
```

Dengan demikian:

```text
GPT
Claude
Gemini
local LLM
company model
future model
human
```

semuanya adalah **external actors**.

---

# 41. IDE Freedom

Wolio tidak menyediakan IDE khusus pada V1.

Wolio menyediakan:

```text
CLI
LSP
WTP
compiler diagnostics
formatter
semantic inspection
```

IDE bebas membangun integration layer.

---

# 42. Human Coding

Wolio tetap harus dapat digunakan tanpa AI.

Contoh:

```bash
vim src/payment/create.wol
wolio check
wolio test
wolio verify
wolio build
```

AI-first ≠ AI-required.

---

# 43. Language Server

LSP V1 menyediakan:

```text
diagnostics
completion
go to definition
find references
rename
hover
semantic information
```

Advanced semantic operations dapat ditambahkan melalui WTP.

---

# 44. Concurrency

Concurrency V1 harus memiliki foundation tetapi tidak memaksakan seluruh advanced model.

Prinsip utama:

```text
shared mutable state
    ↓
explicit
```

Parallel execution harus dapat dianalisis berdasarkan semantic dependency.

Contoh:

```text
load_user()
load_products()
load_promotions()
```

dapat dieksekusi paralel jika tidak memiliki conflicting state.

AI tidak menentukan thread scheduling.

Runtime/compiler yang menentukan execution strategy.

---

# 45. Native Compilation

Target V1:

```text
Wolio
 ↓
Wolio IR
 ↓
LLVM IR
 ↓
Native binary
```

Target awal:

```text
Linux x86_64
```

Architecture lain dapat ditambahkan kemudian.

---

# 46. Runtime

Runtime V1 harus minimal.

Runtime hanya menyediakan primitive yang benar-benar dibutuhkan language semantics.

Tidak boleh menjadi tempat arbitrary business logic.

---

# 47. Security Boundary

Security architecture V1 harus memastikan:

```text
secret
filesystem
network
process
environment
database
```

merupakan capabilities/effects yang dapat ditelusuri.

---

# 48. Architecture Contract

Project dapat menetapkan policy seperti:

```text
payment
    may depend on
        pricing
        customer
        ledger

payment
    must not depend on
        presentation
```

Architecture violation harus dapat dianalisis sebelum merge/build berdasarkan project policy.

---

# 49. Semantic Graph

Semantic graph diturunkan dari:

```text
AST
types
contracts
operations
dependencies
effects
tests
invariants
```

Graph bukan database manual yang harus dipelihara AI.

Ia merupakan derived representation.

---

# 50. Repository History

V1 dapat menyimpan semantic history:

```text
change
actor
scope
before
after
verification
```

Tujuannya:

```text
why did this change happen?
what changed?
what was verified?
what was affected?
```

---

# 51. Source of Truth

Urutan authority:

```text
Source
  ↓
Parser
  ↓
AST
  ↓
Semantic Model
  ↓
Verification
  ↓
IR
  ↓
Native
```

`.wolio/` adalah cache/index.

WTP adalah interface.

LLM bukan source of truth.

Generated semantic graph bukan source of truth.

---

# 52. Error Taxonomy

Error code harus dikelompokkan:

```text
WOLIO-SYNTAX-xxx
WOLIO-TYPE-xxx
WOLIO-SEMANTIC-xxx
WOLIO-CONTRACT-xxx
WOLIO-EFFECT-xxx
WOLIO-DEPENDENCY-xxx
WOLIO-SCOPE-xxx
WOLIO-VERIFY-xxx
WOLIO-PACKAGE-xxx
WOLIO-PROTOCOL-xxx
```

---

# 53. Repository Testing

Setiap crate minimal memiliki:

```text
unit tests
integration tests
golden fixtures
negative fixtures
```

Compiler harus memiliki test untuk:

```text
valid programs
invalid programs
diagnostic stability
semantic stability
```

---

# 54. Golden Fixtures

Repository harus menyediakan contoh:

```text
fixtures/
├── valid/
├── invalid/
├── contracts/
├── effects/
├── dependencies/
├── semantic/
├── diagnostics/
├── agent/
└── regressions/
```

Setiap bug yang pernah ditemukan dapat menjadi regression fixture.

---

# 55. AI Coding Benchmark

Wolio harus diuji melawan bahasa lain.

Baseline:

```text
AI + Go
AI + Rust
AI + TypeScript
AI + Wolio
```

Metric:

```text
task success
semantic bugs
regression
compiler errors
repair iterations
token usage
time
security bugs
concurrency bugs
```

Compiler error reduction saja bukan success metric.

Blueprint sebelumnya sudah menempatkan benchmark ini sebagai bagian penting sebelum implementasi penuh.

---

# 56. Implementation Milestones

## Milestone 1 — Vertical Slice

Target:

```bash
wolio init hello
wolio check
```

Pipeline:

```text
lexer
parser
AST
semantic ID
basic semantic model
CLI
```

---

## Milestone 2 — Semantic Core

Tambahkan:

```text
types
resolver
contracts
effects
dependencies
diagnostics
```

---

## Milestone 3 — Verification

Tambahkan:

```text
verification
tests
impact analysis
semantic graph
invariants
```

---

## Milestone 4 — WTP

Tambahkan:

```text
semantic inspection
scope
change planning
structured mutation
verification
```

---

## Milestone 5 — Native

Tambahkan:

```text
Wolio IR
LLVM IR
runtime
native executable
```

---

## Milestone 6 — Ecosystem

Tambahkan:

```text
package manager
lockfile
LSP
IDE integration
benchmark suite
```

Pendekatan vertical slice ini lebih aman daripada membangun lexer → parser → LLVM secara linear sampai selesai.

---

# 57. Definition of Done — Implementation Specification v1

Wolio v1 dianggap implementable apabila:

### Language

```text
[ ] source dapat diparse
[ ] static typing bekerja
[ ] Result/Option bekerja
[ ] exhaustive matching bekerja
[ ] semantic types bekerja
```

### Semantic

```text
[ ] semantic IDs stabil
[ ] semantic repository bekerja
[ ] dependency graph bekerja
[ ] semantic graph bekerja
```

### Contract

```text
[ ] contract dapat dideklarasikan
[ ] precondition diverifikasi
[ ] postcondition dapat diverifikasi
[ ] effect dapat diverifikasi
[ ] dependency dapat diverifikasi
```

### Verification

```text
[ ] impact analysis bekerja
[ ] test verification bekerja
[ ] invariant verification bekerja
[ ] diagnostics machine-readable
```

### AI/Tooling

```text
[ ] WTP inspect bekerja
[ ] WTP plan bekerja
[ ] WTP scope bekerja
[ ] WTP mutation bekerja
[ ] WTP verify bekerja
```

### Build

```text
[ ] Wolio IR bekerja
[ ] LLVM backend bekerja
[ ] native binary dapat dibuat
```

### Developer Experience

```text
[ ] wolio init
[ ] wolio check
[ ] wolio test
[ ] wolio verify
[ ] wolio build
[ ] LSP dasar
```

---

# 58. Non-Goals of v1

Wolio v1 tidak mencoba menyelesaikan:

```text
general-purpose formal verification
full autonomous AI coding
perfect business correctness
automatic requirement inference
universal concurrency proof
distributed verification
all IDE integrations
all CPU architectures
all programming paradigms
```

Hal ini penting.

Wolio tidak boleh mengklaim bahwa compiler dapat mengetahui apakah algoritma bisnis benar hanya karena code memenuhi contract. Kritik desain sebelumnya sudah menunjukkan bahwa AI dapat menghasilkan implementation yang 100% valid tetapi tetap salah secara business logic.

---

# 59. Fundamental Limitation

Wolio tidak dapat menjamin:

```text
"AI cannot make a logically wrong decision."
```

Wolio menargetkan:

```text
AI cannot silently violate
known constraints.
```

dan:

```text
AI cannot make uncontrolled changes
without detection.
```

serta:

```text
AI changes can be inspected,
scoped, analyzed and verified.
```

Ini adalah batas realistis sistem.

---

# 60. Final Architecture

Model akhir Wolio:

```text
                         HUMAN
                           │
                           │
                    REQUIREMENT / INTENT
                           │
                           ▼
                    ┌───────────────┐
                    │  WOLIO PROJECT │
                    └───────┬───────┘
                            │
              ┌─────────────┴─────────────┐
              │                           │
           HUMAN IDE                 AI / AGENT
              │                           │
              └─────────────┬─────────────┘
                            │
                     Wolio Tool Protocol
                            │
                            ▼
                    Semantic Repository
                            │
              ┌─────────────┼─────────────┐
              │             │             │
              ▼             ▼             ▼
           Contract       Graph       Change Scope
              │             │             │
              └─────────────┼─────────────┘
                            ▼
                      Impact Analysis
                            │
                            ▼
                       Verification
                            │
                    ┌───────┴───────┐
                    ▼               ▼
                  PASS             FAIL
                    │               │
                    ▼               ▼
                 Wolio IR       Diagnostics
                    │               │
                    ▼               │
                  LLVM              │
                    │               │
                    ▼               │
               Native Code          │
                                    │
                                    └──→ AI / IDE
```

---

# 61. The Most Important Architectural Rule

Wolio **tidak memiliki AI**.

Wolio **tidak memilih AI**.

Wolio **tidak memiliki IDE wajib**.

Wolio **tidak bergantung pada vendor AI**.

Wolio menyediakan:

```text
Language
Compiler
Semantic Model
Verification
Tool Protocol
LSP
CLI
```

AI hanyalah salah satu consumer.

---

# 62. Final Mental Model

Jangan membayangkan:

```text
Wolio
= programming language for AI
```

Lebih tepat:

```text
Wolio
=
Programming Language
+
Semantic Software Model
+
Compiler
+
Verification Engine
+
Development Protocol
```

Sedangkan:

```text
AI
+
IDE
+
Human
+
CI
+
Automation
```

adalah **external actors**.

Dengan model ini, seseorang dapat menggunakan Wolio dengan:

```text
Claude + VS Code
```

atau:

```text
Codex + terminal
```

atau:

```text
Gemini + JetBrains
```

atau:

```text
local LLM + custom IDE
```

atau bahkan:

```text
vim + manusia
```

dan semuanya tetap menggunakan compiler dan verification model yang sama.

---

# 63. Implementation Rule for AI Coding

AI coding agent yang mengimplementasikan Wolio sendiri harus menerima task sekecil:

```text
Implement Milestone 1 / Task 001
```

bukan:

```text
Build Wolio compiler.
```

Setiap implementation task harus mendefinisikan:

```text
Purpose
Scope
Files
Dependencies
Public API
Input
Output
Errors
Invariants
Forbidden behavior
Tests
Acceptance criteria
```

Format granular ini sudah menjadi pola yang direkomendasikan oleh technical specification sebelumnya.

---

# 64. End State

Target akhirnya bukan sekadar:

```bash
wolio build
```

Tetapi workflow:

```text
User:
"Tambahkan idempotency ke payment creation."

        ↓

AI:
inspect payment.create

        ↓

WTP:
contract
dependencies
effects
callers
tests
invariants

        ↓

AI:
create change plan

        ↓

Wolio:
impact analysis

        ↓

AI:
apply scoped mutation

        ↓

Wolio:
type check
contract check
effect check
dependency check
test
invariant
semantic analysis

        ↓

PASS

        ↓

Wolio IR
        ↓
LLVM
        ↓
Native
```

Dan bila gagal:

```text
FAIL
 ↓
structured diagnostic
 ↓
allowed repair
 ↓
AI
 ↓
new proposal
```

Bukan:

```text
AI
 ↓
edit 37 files
 ↓
"hopefully works"
```

---

# 65. Specification Status

```text
Implementation Specification v1

Status:
    Architecture baseline

Purpose:
    First implementation contract

Does NOT define:
    specific AI provider
    specific IDE
    specific LLM
    autonomous agent requirement

Defines:
    language implementation
    compiler implementation
    semantic model
    verification
    WTP
    CLI
    package system
    LSP foundation
    native compilation
```

**This specification is the implementation baseline, not the complete limit of Wolio.**

The complete future direction remains in the Blueprint.
