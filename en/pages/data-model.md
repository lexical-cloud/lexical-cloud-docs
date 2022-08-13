---
title: "Data Model"
linkTitle: "Data Model"
weight: 20
---

The docs of Lexical.cloud curate cloud products into hierarchical categories known as a taxonomy.
Taxonomy terms are populated by metadata from each doc and glossary entry.

{{< alert title="Help us improve!" >}}
Use "Create docs issue" to submit ideas for improving the below illustrations. Thanks!
{{< /alert >}}

## Taxonomy Entities

Terms for each entity originate in the docs. The glossary independently defines a term.

```mermaid
erDiagram
    GLOSSARY |o..|| SERVICE : defines
    GLOSSARY |o..|| PROVIDER : defines
    GLOSSARY |o..|| DOMAIN : defines
    GLOSSARY |o..|| CATEGORY : defines
    GLOSSARY |o..|| FEATURE : defines
    GLOSSARY |o..|| LABEL : defines
    SERVICE ||--o{ DOC : originates
    PROVIDER ||--o{ DOC : originates
    DOMAIN ||--o{ DOC : originates
    CATEGORY ||--o{ DOC : originates
    FEATURE ||--o{ DOC : originates
    LABEL ||--o{ DOC : originates
```

| Entity | Description | Example Terms |
| ------ | ---------- | ------- |
| Service | Purpose of a cloud product | Monitor, Governance  |
| Provider | Originator of a cloud product | AWS, Azure, Google Cloud |
| Domain | Collection of services with intersecting categories | Observability, Systems management |
| Category | Group specific to a domain and/or service | Cost management, Dashboards |
| Feature | Specific functionality from a product | Alerts, Reports |
| Label | Attribute of a product | Deprecated |


## Example Metadata

Let's explore **doc** entries for monitoring products:

{{< tabpane >}}
{{< tab header="Amazon CloudWatch" >}}
{{< readfile file="docs/monitor/aws/cloudwatch.md" code="true" lang="yaml" >}}
{{< /tab >}}
{{< tab header="Azure Monitor" >}}
{{< readfile file="docs/monitor/azure/monitor.md" code="true" lang="yaml" >}}
{{< /tab >}}
{{< tab header="Google Cloud Monitoring" >}}
{{< readfile file="docs/monitor/gcp/cloud-monitoring.md" code="true" lang="yaml" >}}
{{< /tab >}}
{{< /tabpane >}}

And **glossary** entries that support them:

{{< tabpane >}}
{{< tab header="Observability" >}}
{{< readfile file="glossary/observability.md" code="true" lang="yaml" >}}
{{< /tab >}}
{{< tab header="Telemetry Data" >}}
{{< readfile file="glossary/telemetry-data.md" code="true" lang="yaml" >}}
{{< /tab >}}
{{< tab header="Open Telemetry" >}}
{{< readfile file="glossary/open-telemetry.md" code="true" lang="yaml" >}}
{{< /tab >}}
{{< tab header="Alerts" >}}
{{< readfile file="glossary/alerts.md" code="true" lang="yaml" >}}
{{< /tab >}}
{{< /tabpane >}}


## Term Relations

The glossary optionally relates each term to ancestor entities. Many docs reference each term.

```mermaid
erDiagram
    GLOSSARY |o..|| SERVICE : defines
    GLOSSARY |o..|| PROVIDER : defines
    GLOSSARY |o..|| DOMAIN : defines
    GLOSSARY |o..|| CATEGORY : defines
    GLOSSARY |o..|| FEATURE : defines
    GLOSSARY |o..|| LABEL : defines
    SERVICE ||--o{ DOC : relates
    PROVIDER ||--o{ DOC : creates
    DOMAIN ||--o{ DOC : relates
    CATEGORY ||--o{ DOC : relates
    FEATURE ||--o{ DOC : relates
    LABEL ||--o{ DOC : describes
    DOMAIN }|--o{ DOMAIN : relates
    DOMAIN ||--o{ SERVICE : relates
    CATEGORY ||--o{ DOMAIN : relates
    CATEGORY }|--o{ CATEGORY: relates
    FEATURE ||--o{ CATEGORY : relates
```

| Entity | Ancestors | Descendants |
| ------ | ---------- | ------- |
| Service | N/A | Domain, Category, Feature |
| Provider | N/A | N/A |
| Domain | Domain | Service, Category, Feature |
| Category | Domain, Service, Category  | Feature |
| Feature | Domain, Service, Category  | N/A |
| Label | N/A  | N/A |


## Logical Data Model

The nested structure of the data is not great for illustrating in ER diagrams.

```mermaid
erDiagram
    GLOSSARY |o..|| PROVIDER : defines
    GLOSSARY |o..|| SERVICE : defines
    GLOSSARY |o..|| DOMAIN : defines
    GLOSSARY |o..|| CATEGORY : defines
    GLOSSARY |o..|| FEATURE : defines
    GLOSSARY |o..|| LABEL : defines
    GLOSSARY {
        string title
        string linkTitle PK
        url definitionLink
    }
    SERVICE }|--|{ DOC : relates
    SERVICE }o--o{ DOMAIN : relates
    SERVICE {
        string term PK
    }
    PROVIDER }|--|{ DOC : creates
    PROVIDER {
        string term PK
    }
    DOMAIN }|--|{ DOC : relates
    DOMAIN }o--o{ CATEGORY : relates
    DOMAIN }|--o{ DOMAIN : relates
    DOMAIN {
        string term PK
        array services FK
        array domains FK
    }
    CATEGORY }|--|{ DOC : relates 
    CATEGORY }o--o{ FEATURE : relates
    CATEGORY }|--o{ CATEGORY: relates
    CATEGORY {
        string term PK
        array services FK
        array domains FK
        array categories FK
    }
    FEATURE }|--o{ DOC : relates 
    FEATURE {
        string term PK
        array services FK
        array domains FK
        array categories FK
    }
    LABEL }|--o{ DOC : describes
    LABEL {
        string term PK
    }
    DOC {
        string title
        string linkTitle
        array services FK
        array domains FK
        array categories FK
        array features FK
        array labels FK
    }
```

## Physical Data Model

Each doc specifes minimal taxonomy terms and infers relationships from the glossary.

```mermaid
erDiagram
    GLOSSARY }o--o{ GLOSSARY : relates
    GLOSSARY {
        string title "Full title of term"
        string linkTitle PK "Term name as referenced by docs"
        url definitionLink "Optional link to definition"
        array services FK "List of terms for related services"
        array domains FK "List of terms for related domains"
        array categories FK "List of terms for related categories"
    }
    GLOSSARY }o..o{ DOC : relates
    DOC {
        string title "Full title of entry"
        string linkTitle "Short title of entry"
        array services FK "List of terms for related services"
        array domains FK "List of terms for related domains"
        array categories FK "List of terms for related categories"
        array features FK "List of terms for related features"
        array labels FK "List of terms for related labels"
    }
```
