---
title: "Data Model"
linkTitle: "Data Model"
weight: 20
---

Lexical.cloud curates cloud products into hierarchical groups, also known as a taxonomy.
Taxonomy terms are populated by metadata in each product, product tier and glossary entry.

| Entry Type | Description | [Directory in product catalog](https://github.com/lexical-cloud/lexical-cloud-docs/en/) |
| ---------- | ----------- | ------- |
| Product | Documented cloud products | products/{service}/{provider}/ | 
| Product Tier | Tiers of cloud products | products/{service}/{provider}/{product}/{tier}/{tierProduct} | 
| Glossary | Terms in product taxonomy | glossary/ |

{{< alert title="Help us improve!" >}}
Use "Create docs issue" to submit ideas for improving the below illustrations. Thanks!
{{< /alert >}}

## Taxonomy Group

Terms for each group originate in products. The glossary independently defines a term.

```mermaid
erDiagram
    GLOSSARY |o..|| SERVICE : defines
    GLOSSARY |o..|| PROVIDER : defines
    GLOSSARY |o..|| DOMAIN : defines
    GLOSSARY |o..|| CATEGORY : defines
    GLOSSARY |o..|| FEATURE : defines
    GLOSSARY |o..|| LABEL : defines
    SERVICE ||--o{ PRODUCT : originates
    PROVIDER ||--o{ PRODUCT : originates
    DOMAIN ||--o{ PRODUCT : originates
    CATEGORY ||--o{ PRODUCT : originates
    FEATURE ||--o{ PRODUCT : originates
    FEATURE ||--o{ PRODUCT-TIER : originates
    LABEL ||--o{ PRODUCT : originates
    LABEL ||--o{ PRODUCT-TIER : originates
    PRODUCT ||--o{ PRODUCT-TIER : has
    PRODUCT-TIER ||--o{ PRODUCT-TIER : has
```

| Group | Description | Example Terms |
| ------ | ---------- | ------- |
| Service | Purpose of a cloud product | Monitor, Governance  |
| Provider | Originator of a cloud product | AWS, Azure, Google Cloud |
| Domain | Collection of services with intersecting categories | Observability, Modernization |
| Category | Group specific to a domain and/or service | Auditing, Dashboards |
| Feature | Specific functionality from a product | Alerts, Reports |
| Label | Attribute of a product | Deprecated |


## Example Metadata

Let's explore **product** entries for monitoring products:

{{< tabpane >}}
{{< tab header="Amazon CloudWatch" >}}
{{< readfile file="products/monitor/aws/cloudwatch.md" code="true" lang="yaml" >}}
{{< /tab >}}
{{< tab header="Azure Monitor" >}}
{{< readfile file="products/monitor/azure/monitor.md" code="true" lang="yaml" >}}
{{< /tab >}}
{{< tab header="Google Cloud Monitoring" >}}
{{< readfile file="products/monitor/gcp/cloud-monitoring.md" code="true" lang="yaml" >}}
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

Notice the relationship of glossary `linkTitle` with the product entries.

##### What about **product tier** entries?

The products above did not have any product tiers, but examples would be:

{{< tabpane >}}
{{< tab header="Model" >}}
---
features:
  - "example feature"
labels:
  - "example label"
title: "Product Name (model name)"
linkTitle: "Model Name"
productHierarchyTier: "model"
---
{{< /tab >}}
{{< tab header="Component" >}}
---
features:
  - "example feature"
labels:
  - "example label"
title: "Product Component Name"
linkTitle: "Component Name"
productHierarchyTier: "component"
---
{{< /tab >}}
{{< /tabpane >}}

Notice that services, categories and domains were not listed. They're inherited from the product. It's possible to list additional entries for all groups, but that is not illustrated on this page.

**Products vs Product Tiers**

| Difference | Product | Product Tier |
| ---------- | ------- | ------------- |
| Location | `{product-path}/product-id.md` | `{product-path}/product-id/tier-id/tier-product-id.md` |
| Attribute | N/A | `productHierarchyTier="model" or "component"` | 
| Groups | can inherit from glossary | also inherits from product |   


**Models vs Components**

| Difference | Model | Component |
| ---------- | ------- | ------------- |
| Purpose | Types of a product | Sub-products |
| Nesting | Under product or component | Under product |


## Term Relations

The glossary optionally relates each term to ancestor groups. Many products reference each term.

```mermaid
erDiagram
    PRODUCT ||--o{ PRODUCT-TIER : has
    GLOSSARY |o..|| SERVICE : defines
    GLOSSARY |o..|| PROVIDER : defines
    GLOSSARY |o..|| DOMAIN : defines
    GLOSSARY |o..|| CATEGORY : defines
    GLOSSARY |o..|| FEATURE : defines
    GLOSSARY |o..|| LABEL : defines
    SERVICE ||--o{ PRODUCT : relates
    PROVIDER ||--o{ PRODUCT : creates
    DOMAIN ||--o{ PRODUCT : relates
    CATEGORY ||--o{ PRODUCT : relates
    FEATURE ||--o{ PRODUCT : relates
    FEATURE ||--o{ PRODUCT-TIER : relates
    LABEL ||--o{ PRODUCT : describes
    LABEL ||--o{ PRODUCT-TIER : describes
    DOMAIN }|--o{ DOMAIN : relates
    DOMAIN ||--o{ SERVICE : relates
    CATEGORY ||--o{ DOMAIN : relates
    CATEGORY }|--o{ CATEGORY: relates
    FEATURE ||--o{ CATEGORY : relates
```

| Group | Ancestors | Descendants |
| ------ | ---------- | ------- |
| Service | N/A | Domain, Category, Feature |
| Provider | N/A | N/A |
| Domain | Domain | Service, Category, Feature |
| Category | Domain, Service, Category  | Feature |
| Feature | Domain, Service, Category  | N/A |
| Label | N/A  | N/A |

Domain and Category can have subgroups. i.e. `container orchestrator` and `kubernetes`

## Logical Data Model

Terms referenced in product entries inhert their ancestors from the glossary.

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
    SERVICE }|--|{ PRODUCT : relates
    SERVICE }o--o{ DOMAIN : relates
    SERVICE {
        string term PK
    }
    PROVIDER }|--|{ PRODUCT : creates
    PROVIDER {
        string term PK
    }
    DOMAIN }|--|{ PRODUCT : relates
    DOMAIN }o--o{ CATEGORY : relates
    DOMAIN }|--o{ DOMAIN : relates
    DOMAIN {
        string term PK
        array services FK
        array domains FK
    }
    CATEGORY }|--|{ PRODUCT : relates 
    CATEGORY }o--o{ FEATURE : relates
    CATEGORY }|--o{ CATEGORY: relates
    CATEGORY {
        string term PK
        array services FK
        array domains FK
        array categories FK
    }
    FEATURE }o--o{ PRODUCT : relates 
    FEATURE }o--o{ PRODUCT-TIER : relates 
    FEATURE {
        string term PK
        array services FK
        array domains FK
        array categories FK
    }
    LABEL }o--o{ PRODUCT : describes
    LABEL }o--o{ PRODUCT-TIER : describes
    LABEL {
        string term PK
    }
    PRODUCT {
        string title
        string linkTitle
        array services FK
        array providers FK
        array domains FK
        array categories FK
        array features FK
        array labels FK
    }
    PRODUCT ||--o{ PRODUCT-TIER : relates
    PRODUCT-TIER ||--o{ PRODUCT-TIER : relates
    PRODUCT-TIER {
        string title
        string linkTitle
        string productHierarchyTier
        string parentProductTier
        array features FK
        array labels FK
    }
```
The model flexibility incurs some complexity and potential of duplicate relationships.

## Physical Data Model

Each product specifes minimal taxonomy terms and infers relationships from the glossary.

```mermaid
erDiagram
    GLOSSARY }o--o{ GLOSSARY : relates
    GLOSSARY {
        string title "Full title of term"
        string linkTitle PK "Term name as referenced by products"
        url definitionLink "Optional link to definition"
        array services FK "List of terms for related services"
        array domains FK "List of terms for related domains"
        array categories FK "List of terms for related categories"
    }
    GLOSSARY }o..o{ PRODUCT : relates
    PRODUCT ||--o{ PRODUCT : "has tiers"
    PRODUCT {
        string productHierarchyPath PK "Nested path to product or tier"
        string title "Full title of entry"
        string linkTitle "Short title of entry"
        array services FK "List of terms for related services"
        array providers FK "List of terms for related providers"
        array domains FK "List of terms for related domains"
        array categories FK "List of terms for related categories"
        array features FK "List of terms for related features"
        array labels FK "List of terms for related labels"
        string productHierarchyTier "Optional tier of product hierarchy "
    }
```

Each product and tier entry should minimally list or inherit a service, domain and category. The `productHierarchyPath` is not an actual column but the physical path in the directory tree.
