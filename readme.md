# RuleBook Wiki: Knowledge Base and Practical Manual

> This is a knowledge-base structure.
> Replace all bracketed placeholders when preparing an article. Mark incomplete sections as “Not documented”.

**Navigation:** [Knowledge Map](#knowledge-map) · [XML Element Reference](#xml-element-reference)

## Knowledge Map

The topics below suggest a breakdown and further development of the knowledge base and are intended as recommendations.

| Section | Proposed articles | Link / readiness |
|---|---|---|
| 01. Fundamentals and architecture | Author and runtime; Collection, Library, Product; Project and Folder; Scheme; dependencies and references | [Add] |
| 02. Data model | DataDef; DataDefData; groups and elements; types; repeating structures; inheritance; default values; paths and context | [Add] |
| 03. Tables and reference data | MatrixDef; Matrix; RefData; lookup; keys; effective dates; missing and duplicate records | [Add] |
| 04. Rules and calculations | RuleSet; input and output; sequencing; conditions; loops; Locate; null; division by zero; rounding | [Add] |
| 05. Forms and interface | Form; Binding; Grid, Row, Column, Cell; tabs; Label; toggle; dropdown; repeating groups; Visibility; Commands; Validation | [Add] |
| 06. EL/PL business logic | Trade; Wages; Turnover; rates; Premium Rating; Status Rating; Commercial Adjustment; Minimum Premium; Annual / For Period | [Add] |
| 07. Workflow and lifecycle | Action; Process; statuses; New Business; Quote; Bind; Renewal; Cancel; date-based restrictions | [Add] |
| 08. Documents | Word templates; data binding; data preparation rules; generation; template incompatibility diagnostics | [Add] |
| 09. Spreadsheet import/export | Sheets and mapping; repeating rows; Append / Replace; duplicates; validation; import result verification | [Add] |
| 10. Testing | Rating Tests; Process Tests; Action Tests; Form Tests; Document Tests; Service Tests; expected output; Test Groups; end-to-end | [Add] |
| 11. Diagnostics | Diagnostic Tracing; minimal reproduction; input/output comparison; Scheme context; reference and type errors | [Add] |
| 12. Integrations | Services; API; input/output contracts; error handling; test requests | [Add] |
| 13. Publishing and maintenance | Compilation; installation; environments; packages; version control; regression; rollback | [Add] |
| 14. Reference guides | Glossary; attribute catalog; error catalog; compatibility matrix; FAQ | [Add] |
| 15. XML element reference | Version-scoped element inventory; one page per element; attributes; child elements; examples; version history | [Reference section](#xml-element-reference) |

---

## XML Element Reference

### Items

| Domain | Reference scope | Index status |
|---|---|---|
| RuleSets | Rules, functions, containers, keys, value providers, control flow | [Populate] |
| Data definitions | Roots, groups, items, types, inheritance, metadata | [Populate] |
| Forms | Layout elements, controls, binding, visibility, commands, validation | [Populate] |
| Matrices | Definitions, key/value columns, data structures | [Populate] |
| Documents | Document configuration and data-binding elements | [Populate] |
| Workflow / Schemes | Version-specific configuration elements | [Populate] |
| Tests | Test definitions, input data, comparisons, groups | [Populate] |
| Services / integrations | Version-specific request/response and service elements | [Populate] |

### Elements

| Element | Domain / parent context | Reference page | Version scope |
|---|---|---|---|
| Lookup | RuleSet | [Lookup](elements/Lookup.md) | Source labeled v2.4+ |
| Key | Lookup / Keys  | [Create key.md] | [Version] |
| Keys | Lookup | [Create keys.md] | [Version] |
| [Exact element name] | [Domain / parent] | [Relative link] | [Version] |

### Required Structure for Every Element Page

1. **Description:** purpose, return value, side effects, and distinctions from similarly named elements.
2. **Infobox:** exact XML name, namespace, domain, version scope, dependencies, evidence status.
3. **Syntax:** minimum valid fragment and the surrounding context required to run it.
4. **Attributes:** exact name, type, required/conditional status, default, allowed values, constraints, source.
5. **Child elements:** allowed names, cardinality, order, parent/child restrictions, source.
6. **Behavior:** empty inputs, nulls, multiple results, errors, execution order.
7. **Usage recipes:** input data, XML, expected result, explanation, runtime-test status.
8. **Version history:** additions, changes, deprecations, removals, migration notes, release-note evidence.
9. **Related elements:** alternatives, dependencies, and similarly named elements.
10. **Sources and open questions:** precise references and unresolved inconsistencies.

---

## Recommended File Organization
This is a proposed structure for the future knowledge base. This file does not automatically create the listed pages.

```text
rulebook-wiki/
  README.md
  assets/
    Lookup_1.png
    Lookup_2.png
    AsPercent_1.png
  elements/
    Lookup.md
    AsPercent.md
  items/
    Matrix.md
    MatrixDef.md
    RuleSet.md
  processes/
    testing.md
    troubleshooting.md
    deployment.md
```

**Cross-references:** when splitting the knowledge base into files, replace textual placeholders with relative Markdown links to real pages. Update the Lookup link if moving the supplied standalone article into the proposed directory tree.
