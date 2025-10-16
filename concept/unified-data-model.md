# Qorix Data-Model(qor-dm) based on IFEX Concept Document
### Unified, Schema-Agnostic Data Model for Application & Communication Design
*(Supporting Qorix Adaptive and Performance Middleware)*

---

## 1️ Vision & Motivation

Modern automotive software architectures face deep fragmentation:

- **Qorix Adaptive** defines rich service-oriented data models, but its exchange format (ARXML) is verbose, tightly coupled to tools, and difficult to evolve.
- **Qorix Performance** provides lightweight runtime abstractions but lacks a unifying meta-model for service and application design.
- Both ecosystems describe *identical logical concerns* — applications, services, interfaces, ports, methods, events, processes, and communication bindings — yet developers must maintain multiple incompatible formats.

**Qorix Data-Model** introduces a *unified, human-readable data model* that bridges these domains build on top of **IFEX (Interface Exchange Format)**.

It is:
- **Schema-agnostic:** Independent of middleware, usable for SOME/IP, DDS, or gRPC.
- **Tool-friendly:** Based on YAML/JSON, easily validated and edited in VS Code.
- **Composable:** Layered model (Core, Machine, Deployment, Behavior).
- **Mappable:** Compatible with Qorix Adaptive and Performance concepts.

---

## 2️ What qor-dm Adds to IFEX

| Layer | Concern | Adaptive Equivalent | S-Core Equivalent | qor-dm Contribution |
|--------|----------|------------------------------|-------------------|-------------------|
| **Core IFEX IDL** | Logical API | `<SERVICE-INTERFACE>`, `<METHOD>`, `<EVENT>` | Interface definition (.json/.idl) | Unified YAML schema, clear semantics |
| **Machine** | Execution topology | `<MACHINE-MANIFEST>` | Node topology manifest | Network and device structure unified |
| **Deployment** | Runtime wiring & QoS | `<EXECUTION-MANIFEST>`, `<SERVICE-INSTANCE>` | Deployment model | Generic transport and service binding |
| **Behavior** | Execution logic | `<APPLICATION-DESIGN>` | Execution graph | Declarative orchestration layer |

The result: an **intuitive, unified design model** spanning both ecosystems.

---

## 3️ Core Concept — Schema-Agnostic Abstraction

IFEX abstracts **intent** instead of syntax.

| AUTOSAR ARXML | Performance JSON | qor-dm YAML |
|---------------|-------------|------------|
| `<METHOD SHORT-NAME="GetRadarFrame">...</METHOD>` | `{ "method": "GetRadarFrame" }` | `- name: get_frame` |

a. Readable  
b. Middleware-independent  
c. Convertible across toolchains

---

## 4️ Unified Application & Communication Design

| Concern | qor-dm Layer | Description |
|----------|-------------|-------------|
| **Application logic** | Core IDL + Behavior | Interfaces, datatypes, and runtime logic |
| **Machine** | Machine Manifest | Physical topology, CPUs, OS, networks |
| **Communication** | Deployment | Transport protocols, service IDs, QoS |
| **Dynamic execution** | Behavior | Sequences, concurrency, timing |

By layering, qor-dm separates responsibilities cleanly while enabling automatic composition.

---

## 5️ Design Principles

1. **Layered Composition:** Independent schemas merge by name and type.
2. **Human-Readable but Machine-Strict:** YAML with JSON Schema validation.
3. **Deterministic Merge Semantics:** Stable sort and additive overlay system.
4. **Cross-Middleware Portability:** Shared transport block (`someip`, `grpc`, etc.).
5. **Declarative Behavior:** Control flow graphs replace static function chains.

---

## 6️ Mapping to Qorix Adaptive

| qor-dm Concept | Qorix Adaptive | Notes |
|---------------|------------------|-------|
| `namespace` | `<AR-PACKAGE>` | Logical grouping |
| `interface` | `<SERVICE-INTERFACE>` | Simplified equivalent |
| `methods` | `<METHOD>` | 1:1 mapping |
| `events` | `<EVENT>` | 1:1 mapping |
| `machine` | `<MACHINE-MANIFEST>` | Identical structure |
| `process` | `<EXECUTION-MANIFEST>` | Process binding |
| `someip.service_id` | `<SERVICE-INTERFACE-IDENTIFIER>` | Preserved |
| `behavior.programs` | `<APPLICATION-DESIGN>` | Generated dynamically |

---

## 7️ Mapping to Qorix Performance

| qor-dm Concept | Performance Concept | Notes |
|---------------|----------------|-------|
| `interface` | Interface JSON | Direct mapping |
| `method` / `event` | Method/Event descriptor | Compatible |
| `machine` | Node descriptor | Network + device data |
| `process` | App descriptor | Matches 1:1 |
| `behavior` | Execution Graph | Aligned to runtime orchestrator |
| `transport` | Binding configuration | SOME/IP, DDS, or IPC |

---

## 8️ Developer Experience

### Before (AUTOSAR ARXML)
```xml
<SERVICE-INTERFACE>
  <SHORT-NAME>RadarData</SHORT-NAME>
</SERVICE-INTERFACE>
```

### After (qor-dm YAML)
```yaml
interface:
  name: radar.v1
  methods:
    - name: get_frame
      input: [{ name: roi, datatype: Roi }]
      returns: [{ name: frame, datatype: bytes }]
```

Readable, maintainable, compatible, and mergeable.

---

## 9️ AI-Assisted Design Integration

- **Schema-driven prompting:** LLMs can reason over structured YAML.
- **Vectorized context:** Each element (method, event, port) is indexable for RAG.
- **Code generation:** Generators can emit C++, Python, or ARXML from the same model.
- **Design validation:** Automated inference of missing connections or mismatched QoS.

---

## 10 Summary — The Unified IFEX Vision

| Aspect |  Adaptive | Performance | qor-dm Unified Layer |
|--------|------------------|--------|---------------------|
| Design Data | ARXML | JSON/YAML | YAML/JSON Schema |
| Application Logic | Service Interfaces | Interface | Core IDL + Behavior |
| Deployment | Execution Manifest | App YAML | Deployment |
| Communication | SOME/IP binding | gRPC/IPC | Transport extensions |
| Behavior | Graphs | Orchestration JSON | Behavior layer |
| Integration | Closed | Open | Schema-driven and tool-neutral |

**In essence:**  
> qor-dm unifies AUTOSAR’s rigor with Qorix Performance’s simplicity, delivering a single schema-driven, human-readable ecosystem for the design, validation, and deployment of service-oriented automotive software.

---

## 11 Outlook

The qor-dm model serves as the foundation for a **model-driven, AI-assisted automotive software platform**.  
It enables:
- Conversion pipelines between ARXML ↔ YAML ↔ Performance JSON.
- Application design editors that visualize behaviors, ports, and processes.
- Context-aware assistants that understand system-level configuration.

**Goal:** Make application and communication design *open, extensible, and intelligent* across the automotive ecosystem.

