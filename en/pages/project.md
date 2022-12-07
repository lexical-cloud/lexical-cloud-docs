---
title: "What is Lexical.cloud?"
linkTitle: "What is Lexical.cloud?"
weight: 20
---

Cloud computing is a vast domain with offerings from many providers. Challenges presented by many providers include:

  * Vernacular differs with each cloud provider
  * Product naming creates distinct vocabularies for each provider
  * Features are distributed differently across products
  * Translation between clouds providers is an art

Lexical.cloud is a cross-reference of products and capabilities from many cloud providers. Goals of this project include:

  * Grouping products for discoverability
  * Identifying capabilities of each product
  * Linking product features with a shared vocabulary

Keep reading below to understand the big picture. The explanation is organized into the following sections:
  * [Context](#context) explains the who and why
  * [Containers](#containers) explains the what
  * [Components](#components) explains the how


## Context

As a `creator`, I want to _develop_ `apps` that _leverage_ the `Lexical.cloud` catalog,
so I can answer questions about cloud resources when choosing an architecture.

As a `consumer`, I want to _explore_ the `Lexical.cloud` catalog,
so I can discover what resources exist to fulfill cloud architecture needs.

As a `contributor`, I want to _maintain_ the `Lexical.cloud` catalog,
so I can spread awareness of options for cloud architecture.

```mermaid
flowchart TD
%% entities
  U1["fa:fa-users Consumers"]
  U2["fa:fa-users Contributors"]
  U3["fa:fa-users Creators"]
%% groups
  subgraph G1["fa:fa-users Users"]
    U1
    U2
    U3    
  end
  subgraph G2["fa:fa-cloud Lexical.cloud
   - Discover resources to fulfill cloud architecture needs.
   - Spread awareness of options for cloud architecture.
  "]
  end
  subgraph G3["fa:fa-box Apps
   - Answer questions about cloud resources when choosing an architecture.
  "]
  end
%% relationships
  U3 -->|develop| G3 -->|leverage| G2
  U1 -->|explore| G2
  U2 -->|maintain| G2
%% styles
  style G1 fill:white,stroke-dasharray:5;
  classDef clickable fill:#3176d9,color:white
%% interactions
  click G2 "#lexicalcloud"
```

### Lexical.cloud

Given `Lexical.cloud` is a `community data` project \
when `contributors` _maintain_ the `community data` \
then `community data` _feeds_ the `frontend` and `backend`.

Given `Lexical.cloud` exposes data in a `backend` \
when `community data` _feeds_ into the `backend` \
then `apps` can _leverage_ the `backend`.

Given `Lexical.cloud` needs a `frontend` to display data \
when `community data` _feeds_ into the `frontend` \
then `consumers` can _explore_ the `frontend`.

```mermaid
flowchart TD
%% entities
  U1["fa:fa-users Consumers"]
  U2["fa:fa-users Contributors"]
  U3["fa:fa-users Creators"]
%% groups
  subgraph G1["fa:fa-users Users"]
    U1
    U2
    U3
  end
  subgraph G2["fa:fa-box Apps"]
  end
  subgraph G3["fa:fa-cloud Lexical.cloud"]
    G4
    G5
    G6
  end
  subgraph G4["fa:fa-database Community Data"]
  end
  subgraph G5["fa:fa-cloud Backend"]
  end
  subgraph G6["fa:fa-cloud Frontend"]
  end
%% relationships
  U1 -->|explore| G6
  U2 -->|maintain| G4 
  U3 -->|develop| G2
  G4 -->|feeds| G5
  G4 -->|feeds| G6
  G2 -->|leverage| G5
%% styles
  classDef clickable fill:#3176d9,color:white
  style G1 fill:white,stroke-dasharray:5;
  style G3 fill:white,stroke:#30638E,stroke-dasharray:10,stroke-width:3px;
%% interactions
  click G4 "#community-data"
  click G5 "#backend"
  click G6 "#frontend"
```

## Containers

### Community Data

Given `Lexical.cloud` `community data` `sources` \
include a `cloud catalog` \
when `contributors` _maintain_ the `cloud catalog` \
then the `cloud catalog` _feeds_ many `transforms`.

Given `Lexical.cloud` `community data` `tranforms` \
include a `static site generator` \
when the `static site generator` _produces_ `html content` \
then the `html content` _populates_ the `frontend` \
and `consumers` can _explore_ that content on the `website`.

Given `Lexical.cloud` `community data` `tranforms` \
include a `json generator` \
when the `json generator` _produces_ the `json data` \
then the `json data` _populates_ updates to the `backend` \
and `apps` _leverage_ services on the `backend`.

```mermaid
flowchart TD
%% entities
  U1["fa:fa-users Consumers"]
  U2["fa:fa-users Contributors"]
  U3["fa:fa-users Creators"]
  I1["fa:fa-table Cloud Catalog"]
  P1["fa:fa-code Static Site Generator"]
  P2["fa:fa-code JSON Generator"]
  O2["fa:fa-bracket JSON Data"]
  O1["fa:fa-globe HTML Content"]
%% groups
  subgraph G1["fa:fa-users Users"]
    U1
    U2
    U3
  end
  subgraph G2["fa:fa-box Apps"]
  end
  subgraph G3["fa:fa-cloud Lexical.cloud"]
    G4
    G5
    G6
  end
  subgraph G4["fa:fa-database Community Data"]
    G4a
    G4b
    G4c
  end
  subgraph G4a["Sources"]
    I1
  end
  subgraph G4b["Transforms"]
    P1
    P2
  end
  subgraph G4c["Outputs"]
    O1
    O2
  end
  subgraph G5["fa:fa-cloud Backend"]
  end
  subgraph G6["fa:fa-cloud Frontend"]
  end
%% relationships
  U1 -->|explore| G6 
  U2 -->|maintain| I1 
  U3 -->|develop| G2
  O1 -->|populates| G6
  O2 -->|populates| G5
  G2 -->|leverage| G5
  I1 -->|feeds| P1 -->|produces| O1 
  I1 -->|feeds| P2 -->|produces| O2
%% styles
  classDef clickable fill:#3176d9,color:white
  style G1 fill:white,stroke-dasharray:5;
  style G3 fill:white,stroke-dasharray:5;
  style G4 fill:white,stroke:#30638E,stroke-dasharray:10,stroke-width:3px;
  style G4a fill:white,stroke-dasharray:5;
  style G4b fill:white,stroke-dasharray:5;
  style G4c fill:white,stroke-dasharray:5;
%% interactions
  click I1 "#cloud-catalog"
  click P1 "#static-site-generator"
  click P2 "#json-generator"
  click O1 "#html-content"
  click O2 "#json-data"
  click G5 "#backend"
  click G6 "#frontend"
```

### Backend

Given `Lexical.cloud` `backend` begins `ingestion` with `storage` \
when `community data` is sent to _stage_ in `storage` \
then data in `storage` is ready to _process_ by the `state machine`.

Given `Lexical.cloud` `backend` persist state in a `datastore` \
when `ingestion` completes at the `state machine` \
then the `state machine` will _update_ the `datastore`.

Given `Lexical.cloud` `backend` `api` is fronted by an `api gateway` \
when `apps` _leverage_ the `api` \
then the `api gateway` will _call_ `functions` \
and the `functions` will _query_ the `datastore`.

```mermaid
flowchart TD
%% entities
  U1["fa:fa-users Consumers"]
  U2["fa:fa-users Contributors"]
  U3["fa:fa-users Creators"]
  I1["fa:fa-floppy-disk Storage"]
  I2["fa:fa-code API Gateway"]
  O1["fa:fa-database Datastore"]
%% groups
  subgraph G1["fa:fa-users Users"]
    U1
    U2
    U3
  end
  subgraph G2["fa:fa-box Apps"]
  end
  subgraph G3["fa:fa-cloud Lexical.cloud"]
    G4
    G5
    G6
  end
  subgraph G4["fa:fa-database Community Data"]
  end
  subgraph G5["fa:fa-cloud Backend"]
    G7
    G8
    O1
  end
  subgraph G6["fa:fa-cloud Frontend"]
  end
  subgraph G7["Ingestion"]
    I1
    G7a
  end
  subgraph G7a["fa:fa-pipe-section State Machine"]
  end
  subgraph G8["API"]
    I2
    G8a
  end
  subgraph G8a["Functions"]
  end
%% relationships
  U1 -->|explore| G6
  U2 -->|maintain| G4
  U3 -->|develop| G2
  G4 -->|stage| G7
  G4 -->|publish| G6
  G2 -->|leverage| G8
  I1 -->|process| G7a
  I2 -->|call| G8a
  G7a -->|update| O1
  G8a -->|query| O1
%% styles
  classDef clickable fill:#3176d9,color:white
  style G1 fill:white,stroke-dasharray:5;
  style G3 fill:white,stroke-dasharray:5;
  style G4 fill:white,stroke-dasharray:5;
  style G5 fill:white,stroke:#30638E,stroke-dasharray:10,stroke-width:3px;
  style G6 fill:white,stroke-dasharray:5;
  style G7 fill:white,stroke-dasharray:5;
  style G8 fill:white,stroke-dasharray:5;
  style O1 fill:#61affe,stroke:white
%% interactions
  click G4 "#community-data"
  click G6 "#frontend"
  click O1 "#datastore"
  click I1 "#storage"
  click I2 "#api-gateway"
  click G7a "#state-machine"
  click G8a "#functions"
```

### Frontend

Given `Lexical.cloud` `frontend` includes a `website` \
when `community data` _populates_ the `frontend` \
then `consumers` can _explore_ that content on the `website`.

```mermaid
flowchart TD
%% entities
  U1["fa:fa-users Consumers"]
  U2["fa:fa-users Contributors"]
  U3["fa:fa-users Creators"]
  O1["fa:fa-globe Website"]
%% groups
  subgraph G1["fa:fa-users Users"]
    U1
    U2
    U3
  end
  subgraph G2["fa:fa-box Apps"]
  end
  subgraph G3["fa:fa-cloud Lexical.cloud"]
    G4
    G5
    G6
  end
  subgraph G4["fa:fa-database Community Data"]
  end
  subgraph G5["fa:fa-cloud Backend"]
  end
  subgraph G6["fa:fa-cloud Frontend"]
    O1
  end
%% relationships
  U1 -->|explore| O1 
  U2 -->|maintain| G4 
  U3 -->|develop| G2
  G4 -->|populates| G5
  G4 -->|populates| G6
  G2 -->|leverage| G5
%% styles
  classDef clickable fill:#3176d9,color:white
  style G1 fill:white,stroke-dasharray:5;
  style G3 fill:white,stroke-dasharray:5;
  style G6 fill:white,stroke:#30638E,stroke-dasharray:10,stroke-width:3px;
%% interactions
  click G4 "#community-data"
  click O1 "#website"
```

## Components

### Cloud Catalog


```mermaid
flowchart LR
%% entities
  C1["fab:fa-github lexical-cloud-docs"]
%% groups
  subgraph G1["fa:fa-table Cloud Catalog"]
    C1
  end
%% relationships
%% styles
  classDef clickable fill:#3176d9,color:white
%% interactions
  click C1 href "https://www.github.com/lexical-cloud/lexical-cloud-docs" _blank
```

### Static Site Generator

```mermaid
flowchart LR
%% entities
  C1["fab:fa-github lexical-cloud-docs"]
  C2["fab:fa-github lexical-cloud-docs-hugo"]
  C3["fab:fa-github docsy"]
  C4["fab:fa-github lexical-cloud.github.io"]
%% groups
  subgraph G1["fa:fa-code Static Site Generator"]
    C2
    C3
  end
%% relationships
  C1 -->|input| C2
  C2 ---|base theme| C3
  C2 -->|output| C4
%% styles
  classDef clickable fill:#3176d9,color:white
%% interactions
  click C1 href "https://www.github.com/lexical-cloud/lexical-cloud-docs" _blank
  click C2 href "https://www.github.com/lexical-cloud/lexical-cloud-docs-hugo" _blank
  click C3 href "https://www.github.com/lexical-cloud/docsy" _blank
  click C4 href "https://www.github.com/lexical-cloud/lexical-cloud.github.io" _blank
```

### HTML Content

```mermaid
flowchart LR
%% entities
  C1["fab:fa-github lexical-cloud.github.io"]
%% groups
  subgraph G1["fa:fa-code HTML Content"]
    C1
  end
%% relationships
%% styles
  classDef clickable fill:#3176d9,color:white
%% interactions
  click C1 href "https://www.github.com/lexical-cloud/lexical-cloud.github.io" _blank
```

### Website

```mermaid
flowchart LR
%% entities
  C1["fab:fa-github Github Pages"]
  C2["fab:fa-github lexical-cloud.github.io"]
%% groups
  subgraph G1["fa:fa-code Website"]
    C1
  end
%% relationships
  C1 -->|serve| C2
%% styles
  classDef clickable fill:#3176d9,color:white
%% interactions
  click C2 href "https://www.github.com/lexical-cloud/lexical-cloud.github.io" _blank
```

### JSON Generator

```mermaid
flowchart LR
%% entities
  C1["fab:fa-github lexical-cloud-docs"]
  C2["fab:fa-github lexical-cloud-data-hugo"]
  C3["fab:fa-github docsy"]
  C4["fab:fa-github lexical-cloud-data"]
%% groups
  subgraph G1["fa:fa-code JSON Generator"]
    C2
    C3
  end
%% relationships
  C1 -->|input| C2
  C2 ---|base theme| C3
  C2 -->|output| C4
%% styles
  classDef clickable fill:#3176d9,color:white
%% interactions
  click C1 href "https://www.github.com/lexical-cloud/lexical-cloud-docs" _blank
  click C2 href "https://www.github.com/lexical-cloud/lexical-cloud-data-hugo" _blank
  click C3 href "https://www.github.com/lexical-cloud/docsy" _blank
  click C4 href "https://www.github.com/lexical-cloud/lexical-cloud-data" _blank
```

### JSON Data

```mermaid
flowchart LR
%% entities
  C1["fab:fa-github lexical-cloud-data"]
%% groups
  subgraph G1["fa:fa-code JSON Data"]
    C1
  end
%% relationships
%% styles
  classDef clickable fill:#3176d9,color:white
%% interactions
  click C1 href "https://www.github.com/lexical-cloud/lexical-cloud-data" _blank
```

### API Gateway  

TODO

### Functions

TODO

### Storage

TODO

### State Machine

TODO

### Datastore

TODO



## Code

TODO

```mermaid
flowchart TD
  lexical-cloud-docs -->|consumed by| lexical-cloud-docs-hugo -->|generates html| lexical-cloud.github.io
  lexical-cloud-docs -->|consumed by| lexical-cloud-data-hugo -->|generates json| lexical-cloud-data
  lexical-cloud-data -->|analyzed by| lexical-cloud-analysis
  lexical-cloud-data -->|ingested by| lexical-cloud-pipeline-___
  lexical-cloud-pipeline-___  -->|served by| lexical-cloud-api-___
  click lexical-cloud-docs href "https://www.github.com/lexical-cloud/lexical-cloud-docs" _blank
  
```