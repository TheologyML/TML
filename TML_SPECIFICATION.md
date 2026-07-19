# Theology Modeling Language Specification

Status: Implementation reference  
Author: TML Flow project  
Last updated: July 18, 2026  
Version: 1.0

This document describes the TML model, diagrams, validation rules, and project serialization used by the current TML Flow application. Planned extensions are listed as reserved decisions, not as empty placeholders.

## Table of contents

- 1. Purpose
- 2. Scope
- 3. Core Principles
- 4. Terminology
- 5. Metamodel Overview
  - 5.1 Abstract Element Types
  - 5.2 Concrete Element Types
  - 5.3 Common Element Fields
- 6. Attributes
  - 6.1 Attribute Types
  - 6.2 Date Rules
- 7. Associations
- 8. Containment Rules
- 9. Diagrams
  - 9.1 Relational Diagram
  - 9.2 Logic Diagram
  - 9.3 Timeline Diagram
  - 9.4 Map Diagram
  - 9.5 Table Diagram
- 10. Textual TML Syntax
- 11. Bible References
- 12. Writings, Quotes, And Citations
- 13. Notes And Wiki Links
- 14. Logic And Argument Modeling
- 15. Historical Modeling
- 16. Validation Rules
- 17. Serialization And Project Files
- 18. Examples
  - 18.1 Doctrine Example
  - 18.2 Argument Example
  - 18.3 Historical Timeline Example
  - 18.4 Writing And Quote Example
  - 18.5 NoteDocument Example
- 19. Reserved Decisions
- 20. Change Log

---

## 1. Purpose

Theology Modeling Language, or TML, is a modeling language for theological, philosophical, textual, logical, and historical work. It gives users a shared model beneath diagrams, tables, and generated textual output so the same doctrine, person, quote, argument, or event can appear in more than one view without becoming duplicated notes.

TML is intended for work that ordinary prose handles awkwardly:

- tracing how claims, doctrines, texts, people, councils, and arguments relate to each other;
- separating a source from a claim made about that source;
- mapping support, entailment, objection, response, contradiction, and definition;
- keeping historical context visible without turning it into a separate timeline document;
- moving between visual diagrams, table views, and textual summaries of the same model.

TML is not intended to replace prose essays, citation managers, formal proof assistants, database schemas, or critical editions. It should help a reader and writer reason through theological material, not pretend that all theological work can be reduced to boxes and arrows.

## 2. Scope

In scope:

- model elements for concepts, texts, authors, history, and logic;
- typed associations with readable semantics and direction;
- containment for organizing complex models;
- project-specific custom node and relationship types;
- visual diagram views, including relational, logic, timeline, map, and table diagrams;
- generated textual TML for inspection, sharing, and interchange previews;
- project saving in `.tmlbundle` files and JSON `.tmlproj` compatibility/import files;
- Bible references, writings, quotes, and citation-derived fields;
- NoteDocument elements, element notes, and wiki-style note links;
- historical year-only dates, date ranges, and known full dates.

Out of scope for the current application version:

- a fully round-trippable textual parser;
- strict theological ontology commitments;
- formal proof checking;
- canonical citation-style rendering beyond derived app metadata;
- first-class uncertainty markers such as `circa`, `before`, or `after`;
- first-class historical periods or eras;
- cross-project package dependency management.

## 3. Core Principles

- TML separates model objects from diagram views.
- A model object can appear in multiple diagrams or tables without becoming a different object.
- Associations are semantic relationships in the model, not only drawn edges.
- Direction matters and should match the relationship's readable meaning.
- Textual sources, historical context, and logical argument should be modeled together.
- Uncertainty should be allowed where historical and interpretive work requires it.
- Derived values should be marked as derived and not treated as user-authored source fields.
- The language must remain useful while reserved decisions are still being settled.

## 4. Terminology

```
Project
  A saved TML Flow model workspace containing project settings, diagrams,
  elements, associations, and file metadata. The app can keep multiple projects
  open in memory. The preferred saved format is .tmlbundle; a JSON .tmlproj file
  is also readable and is the simplest format for generated project imports.

Model
  The semantic graph of elements and associations. Diagrams are views of this graph.

Element
  A typed model object, such as a Person, Writing, Doctrine, Premise, Event, or Quote.

Association
  A typed relationship between two model elements, such as Support, Believe, Define,
  or Contradict.

Diagram
  A visual or tabular view over model elements and associations.

View
  A diagram-specific representation of a model element, association, or annotation.
  Deleting a node view from a diagram does not necessarily delete the model element.

Containment
  An ownership/organization relation from one model element to another. Containment
  is used for packages, writings, arguments, and other structured models.

Attribute
  A named value on an element, such as birthDate, author, text, tradition, or status.

Relationship End
  The association-derived property name visible from either side of a relationship,
  such as supports/supportedBy or defines/definedBy.

Note
  Markdown-like content stored on an element. NoteDocument elements are dedicated
  long-form notes, but every element may carry note text.

Wiki Link
  A note reference to another model element, written in markdown as [[Element Label]]
  and stored structurally in element.note.wikiLinks when known.
```

## 5. Metamodel Overview

TML's metamodel is inheritance-based. Abstract element types define shared attributes and broad categories. Concrete element types are the objects users create.

### 5.1 Abstract Element Types

| Element | Parent | Purpose |
|---|---|---|
| `GhostElement` | none | Base element for theological, philosophical, and historical models. Provides `alternateNames`, `sourceReference`, and derived `showsUpIn`. |
| `Association` | `GhostElement` | Abstract base for relationship-like model elements. Current app associations are stored separately as model associations. |
| `Words` | `GhostElement` | Base element for textual or quoted material. |
| `Author` | `GhostElement` | Base element for a source of writing, speech, or ideas. |

### 5.2 Concrete Element Types

(Selected entries — full table in original spec)

| Element | Parent | Category | Definition | Key Attributes |
|---|---|---|---|---|
| `System` | `GhostElement` | Structure | A full theological or philosophical system. | `tradition`, `scope` |
| `Package` | `GhostElement` | Structure | A container for related model elements. | `purpose` |
| `Writing` | `Words` | Text | A written work, document, book, letter, or treatise. | `author`, derived `citation`, `dateWritten`, `genre`, `language`, PDF metadata |
| `Quote` | `Words` | Text | A quotation from a source. | `author`, derived `citation`, `quotedText`, `sourceWriting`, `page`, `chapter`, `line`, `location`, PDF metadata |
| `Person` | `Author` | History | A person who authors, teaches, believes, or acts. | `birthDate`, `deathDate`, `role`, `location` |
| `Argument` | `GhostElement` | Logic | A structured line of reasoning with premises, conclusions, objections, and replies. | `argumentForm`, `validity`, `summary` |

### 5.3 Common Element Fields

Every model element has:

- `id`: stable project-local identifier;
- `kind`: concrete element kind;
- `label`: display name;
- `attributes`: key/value map governed by the metamodel;
- `category`: display category;
- `parent`: metamodel parent label or kind;
- `containerId`: optional containing element id;
- `notes`: legacy synchronized note text;
- `note`: structured note data with `markdownContent` and `wikiLinks`;
- `mainDiagramId`: optional diagram id to open from the element.

## 6. Attributes

### 6.1 Attribute Types

| Type | Meaning | Validation |
|---|---|---|
| `text` | Single-line string value. | Any string unless a specific attribute defines additional rules. |
| `multiline` | Longer text value. | Any string; rendered with textarea-like editing. |
| `date` | Historical or known calendar date. | Empty, `YYYY`, `YYYY-YYYY`, or `MM/DD/YYYY` with valid month/day/year. |
| `choice` | One value from a defined option set. | Should match one allowed option value. |
| `elementReference` | Reference to another model element by id. | Should point to an existing element of the configured reference kind. |
| `diagramList` | Derived list of diagrams where an element appears. | Derived and read-only. |
| `derived` | Metadata flag for attributes computed from model state. | Users should not edit derived values directly. |

### 6.2 Date Rules

TML date attributes are strings with controlled formats:

```
YYYY
YYYY-YYYY
MM/DD/YYYY
```

Rules:

- Use `YYYY` for historical year-only dates, such as `325`.
- Use `YYYY-YYYY` for ranges where only the start and end years are modeled, such as `680-681`.
- Use `MM/DD/YYYY` only when month and day are known, such as `05/02/373`.
- Years may be negative for BC/BCE-like dates, but year zero is invalid.
- The start year in a range must be less than or equal to the end year.
- Empty date values are valid.
- Legacy `00/00/YYYY` values are accepted for old project compatibility, but new guidance should use `YYYY`.

Timeline diagrams use the start year of a date value. For a range, the start year controls placement.

Reserved date decisions:

- Whether negative years should be displayed as `BC`/`BCE` in the UI.
- Whether approximate forms like `c. 325`, `before 325`, or `after 325` should become first-class values.
- Whether historical periods should be modeled as date values or as Event/Group/Idea elements.

## 7. Associations

Associations are typed edges between model elements. They carry readable semantics, direction, relationship-end names, and diagram defaults.

(Selected association table entries)

| Association | Parent | Direction | Read As | Source End | Target End |
|---|---|---|---|---|---|
| `Affiliated` | `Association` | undirected | Source is affiliated with target. | `affiliatedWith` | `affiliatedWith` |
| `Believe` | `Association` | source-to-target | Source believes target. | `believes` | `believedBy` |
| `Support` | `Converse` | source-to-target | Source supports target. | `supports` | `supportedBy` |
| `Entail` | `Converse` | source-to-target | Source entails target. | `entails` | `entailedBy` |
| `Contradict` | `Converse` | bidirectional | Source contradicts target. | `contradicts` | `contradicts` |

Direction rules:

- `source-to-target` draws the arrow from source to target.
- `target-to-source` draws the arrow from target to source while preserving the source/target reading. `Follow` uses this because "source follows from target."
- `bidirectional` draws arrows both ways.
- `undirected` draws no semantic arrow.

Typical uses and guidance are described in the full spec.

## 8. Containment Rules

Containment is project-local ownership and organization. It may represent semantic containment, such as an Argument containing Premises, or editorial containment, such as a Package grouping materials for a chapter.

General rules:

- Any concrete element may appear at the model root.
- Packages are the preferred general-purpose containers.
- A contained element has one `containerId`.
- Deleting a containing element deletes contained children unless they are moved first.
- Diagrams may be owned by model elements.
- A diagram view can show elements outside the diagram owner's containment tree.

(Containment table present in original spec.)

## 9. Diagrams

Diagrams are views over the model. They contain node views, edge views, annotation views, and type-specific settings. Annotation views are diagram-local comments, visual boxes, map labels, or pasted images; pasted image annotations may store `imageDataUrl`, `imageMimeType`, and `imageName` in project JSON.

### 9.1 Relational Diagram

Purpose: a UML-like relationship diagram for general model structure.

Use for: doctrine maps, source-to-claim maps, people/groups/belief relationships, mixed theological/historical/contextual diagrams.

### 9.2 Logic Diagram

Purpose: an argument-flow diagram for premises, support, objections, replies, and conclusions.

Behavior and recommended patterns are described in the spec. Example arrangement:

```
Question contains Argument
Argument contains Premise
Argument contains Conclusion
Premise --Support--> Conclusion
Quote --Support--> Premise
Objection --Object--> Argument
Reply --Respond--> Objection
```

### 9.3 Timeline Diagram

Purpose: a date-aware historical diagram with a timeline axis.

Timeline diagrams read date-like attributes in priority order: `startDate`, `birthDate`, `foundedDate`, `dateWritten`, `deathDate`, `endDate`.

Behavior notes include positioning by parsed start year and arranging nodes chronologically.

### 9.4 Map Diagram

Purpose: offline world-map diagram for geographic context, movement, and place-based events.

Behavior notes: uses bundled Natural Earth data; map point nodes store normalized lat/long on `nodeView.mapLocation`; routes/regions use `mapOverlays`.

### 9.5 Table Diagram

Purpose: generic table view for model elements and attributes. Supports row/column filtering, sorting, editable cells, CSV export of visible rows.

## 10. Textual TML Syntax

The current app generates textual TML for inspection. The textual form is not yet round-trippable. Examples:

```tml
Doctrine "Trinity" extends ghostElement {
  tradition: "Nicene"
  definition: "The one God exists eternally as Father, Son, and Holy Spirit."
}

Person "Athanasius of Alexandria" extends author {
  birthDate: "296"
  deathDate: "05/02/373"
}
```

Associations example:

```tml
Believe "Athanasius of Alexandria" -> "Trinity"
Support "On the Incarnation 1.1" -> "The Word is creator"
Follow "Conclusion" <- "Premise"
Link "Athanasius of Alexandria" -- "Nicene party"
Contradict "Arian thesis" -> "Nicene confession"
```

Notes: arrows `--`, `<-`, and `->` are used; future textual TML should define stable id syntax and round-trip parsing.

## 11. Bible References

`BibleVerse` elements represent biblical source references. Core attributes: `book`, `chapter`, `verse` (ranges or comma-separated), `translation`, `text`. Canonical display form example:

```
John 1:1-3 WEB
```

Overlap rules: overlap when book, chapter, translation, and verse ranges overlap. Reserved decision: whether translation remains part of identity.

## 12. Writings, Quotes, And Citations

`Writing` is a written work and may reference a `Person` as author and carry PDF metadata. `Quote` is an excerpt that should point back to `sourceWriting` and can store location metadata. Citation fields are derived in-app and should not be edited as primary source data.

## 13. Notes And Wiki Links

Structured note shape (JSON):

```json
{
  "note" : {
	"markdownContent" : "# Heading\n\nA note with [[Trinity]] as a wiki link.",
	"wikiLinks" : [
	  {
		"id" : "wiki-link-1",
		"relationshipTypeId" : "link",
		"targetElementId" : "model-doctrine-1"
	  }
	]
  },
  "notes" : "# Heading\n\nA note with [[Trinity]] as a wiki link."
}
```

Rules: `note.markdownContent` is canonical; `notes` retained for compatibility; wiki links use `[[Element Label]]` or `[[Element Label|Display Text]]`.

## 14. Logic And Argument Modeling

TML distinguishes the structure of reasoning from texts and people. Element types: `Argument`, `Premise`, `Conclusion`, `Objection`, `Reply`, `Question`, `Fallacy`, `Distinction`, `Definition`.

Association guidance: use `Support`, `Entail`, `Assume`, `Follow`, `Link`, `Object`, `Respond`, `Contradict` as appropriate.

## 15. Historical Modeling

Use `Person`, `Group`, `Event`, and dated `Writing` elements. Use year-only dates or ranges for uncertain dates.

## 16. Validation Rules

Validation requirements include date formats, unique ids, existing references for associations and diagram views, annotation view constraints, and other model integrity rules. Non-blocking warnings include semantically unusual associations and missing citation metadata.

## 17. Serialization And Project Files

`.tmlbundle` is preferred: a ZIP-compatible container with `manifest.json`, schema-1 `project.json`, and attachments/. `.tmlproj` JSON files remain supported for compatibility and AI-generated imports. Project JSON may include `version`, `name`, `modelElements`, `modelAssociations`, `diagrams`, and related fields.

Normalization may migrate legacy shapes, fill defaults, repair duplicate ids, and derive read-only attributes.

## 18. Examples

### 18.1 Doctrine Example

```tml
Doctrine "Trinity" extends ghostElement {
  tradition: "Nicene"
  definition: "The one God exists eternally as Father, Son, and Holy Spirit."
}

Person "Athanasius of Alexandria" extends author {
  birthDate: "296"
  deathDate: "05/02/373"
}

Definition "Trinity definition" extends idea {
  term: "Trinity"
  definitionText: "One ousia, three hypostases."
}

Believe "Athanasius of Alexandria" -> "Trinity"
Define "Trinity definition" -> "Trinity"
```

### 18.2 Argument Example

(See original spec for full example code.)

### 18.3 Historical Timeline Example

(See original spec for full example code.)

### 18.4 Writing And Quote Example

(See original spec for full example code.)

### 18.5 NoteDocument Example

(JSON example present in original spec.)

## 19. Reserved Decisions

(Decision table present in original spec listing display of BC dates, approximate date handling, BibleVerse translation identity, textual TML round-trip, association validation strictness, containment typing, and citation styles.)

## 20. Change Log

- July 18, 2026 — 1.0: Revalidated the implementation reference against the authoritative Project store, bundle/schema contracts, five diagram adapters, notes, tables, maps, source workflows, and current validation behavior; no language-level version change.
- July 3, 2026 — 1.0: Renamed the specification document to the implementation reference and marked incomplete product areas as reserved decisions.
- July 3, 2026 — 0.5: Added NoteDocument, structured notes, wiki-link metadata, project workspace tab state, and choice attributes.
- May 31, 2026 — 0.4: Added Map diagrams, role-aware Logic diagram flow, pasted image annotations, linked PDF replacement, and map/annotation serialization notes.
- May 27, 2026 — 0.3: Added project-specific custom types and clarified that new app saves use `.tmlbundle` while `.tmlproj` remains readable/importable.
- May 25, 2026 — 0.2: Clarified `.tmlbundle` as the preferred saved format and `.tmlproj` as the simple AI-generated import format.
- May 18, 2026 — 0.1: Created the first working specification aligned with the TML Flow metamodel, associations, dates, diagrams, serialization, and examples.

