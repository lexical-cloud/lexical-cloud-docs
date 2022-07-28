---
title: "Data Model"
linkTitle: "Data Model"
weight: 20
---

## Taxonomy Entities

```mermaid
erDiagram
    DOCS ||--|{ SERVICE : provides
    DOCS ||--|{ PROVIDER : from 
    DOCS ||--|{ DOMAIN : belongs
    DOCS ||--|{ CATEGORY : belongs
    DOCS ||--o{ FEATURE : contains
    DOCS ||--o{ LABEL : has
```

## Taxonomy Relations

```mermaid
erDiagram
    DOCS ||--|{ SERVICE : provides
    DOCS ||--|{ PROVIDER : from 
    DOCS ||--|{ DOMAIN : belongs
    DOCS ||--|{ CATEGORY : belongs
    DOCS ||--o{ FEATURE : contains
    DOCS ||--o{ LABEL : has
    DOMAIN ||--o{ DOMAIN : relates
    DOMAIN ||--o{ SERVICE : relates
    CATEGORY ||--o{ DOMAIN : relates
    CATEGORY ||--o{ CATEGORY: relates
    FEATURE ||--o{ CATEGORY : relates
```

## Logical Model

```mermaid
erDiagram
    DOCS ||--|{ SERVICE : provides
    DOCS ||--|{ PROVIDER : from 
    DOCS ||--|{ DOMAIN : belongs
    DOCS ||--|{ CATEGORY : belongs
    DOCS ||--o{ FEATURE : contains
    DOCS ||--o{ LABEL : has
    DOCS {
        string title
        string linkTitle
        array services FK
        array domains FK
        array categories FK
        array features FK
        array labels FK
    }
    GLOSSARY ||--|| PROVIDER : defines
    GLOSSARY ||--|| SERVICE : defines
    GLOSSARY ||--|| DOMAIN : defines
    GLOSSARY ||--|| CATEGORY: defines
    GLOSSARY ||--|| FEATURE : defines
    GLOSSARY ||--|| LABEL : defines
    GLOSSARY {
        string title
        string linkTitle PK
        url definitionLink
    }
    SERVICE {
        string term PK
    }
    PROVIDER {
        string term PK
    }
    DOMAIN ||--o{ SERVICE : relates
    DOMAIN ||--o{ DOMAIN : relates
    DOMAIN {
        string term PK
        array services FK
        array domains FK
    }
    CATEGORY ||--|{ DOMAIN : relates
    CATEGORY ||--o{ CATEGORY: relates
    CATEGORY {
        string term PK
        array services FK
        array domains FK
        array categories FK
    }
    FEATURE ||--|{ CATEGORY : relates
    FEATURE {
        string term PK
        array services FK
        array domains FK
        array categories FK
    }
    LABEL {
        string term PK
    }
```

## Physical Model

```mermaid
erDiagram
    DOCS ||--|{ GLOSSARY : relates
    DOCS {
        string title "Full title of entry"
        string linkTitle "Short title of entry"
        array services FK "List of terms for related services"
        array domains FK "List of terms for related domains"
        array categories FK "List of terms for related categories"
        array features FK "List of terms for related features"
        array labels FK "List of terms for related labels"
    }
    GLOSSARY ||--o{ GLOSSARY : relates
    GLOSSARY {
        string title "Full title of term"
        string linkTitle PK "Term name as referenced by docs"
        url definitionLink "Optional link to definition"
        array services FK "List of terms for related services"
        array domains FK "List of terms for related domains"
        array categories FK "List of terms for related categories"
    }
```
