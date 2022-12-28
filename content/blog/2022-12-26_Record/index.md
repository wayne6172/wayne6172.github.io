---
title: "2022-12-26 Record"
date: 2022-12-26T21:17:32+08:00
categories:
- "Blog Update"
tags:
- Mermaid

mermaid: true

draft: false 
summary: 更新部落格，將tranquilpeak直接fork後，submodule改用fork後的，嘗試引用Mermaid 9.3.0 (原tranquilpeak只支援8.12.1)
---

## Mermaid使用方式

先將Markdown的最上層標籤加入```mermaid: true```，隨後Code區指定mermaid語法，以及加上mermaid的標籤即可

mermaid版本: 9.3.0

### 測試1

```mermaid
{{<mermaid>}}
graph TD
    A[Choosing an OS] --> B{Do you fear technology ?}
    B -->|Yes| C{Is your daddy rich ?}
    C -->|Yes| E(fa:fa-apple Mac OS);
    C -->|No| F(fa:fa-chrome Chrome OS);
    
    B -->|No| D{Do you care about freedom/privacy ?}
    D -->|Yes| G{Do you have a life ?}
    D -->|No| H(fa:fa-windows Windows);
    
    G -->|Yes| I(fa:fa-ubuntu Ubuntu);
    G -->|Yes| K(fa:fa-fedora Fedora);
    G -->|No| L(fa:fa-linux Archlinux);
    G -->|No| M(fa:fa-shield Backtrack);

    style A fill:#0094FF,stroke:#333,stroke-width:4px,color:#fff
    style E fill:#808080,stroke:#333,stroke-width:2px,color:#fff,stroke-dasharray: 5 5
    style F fill:#808080,stroke:#333,stroke-width:2px,color:#fff,stroke-dasharray: 5 5
    style H fill:#004A7F,stroke:#333,stroke-width:2px,color:#fff,stroke-dasharray: 5 5
    style I fill:#FF6A00,stroke:#333,stroke-width:2px,color:#fff,stroke-dasharray: 5 5
    style K fill:#FF6A00,stroke:#333,stroke-width:2px,color:#fff,stroke-dasharray: 5 5
    style L fill:#7F0000,stroke:#333,stroke-width:2px,color:#fff,stroke-dasharray: 5 5
    style M fill:#7F0000,stroke:#333,stroke-width:2px,color:#fff,stroke-dasharray: 5 5
{{< /mermaid >}}
```

### 測試2

```mermaid
{{<mermaid>}}
%%{init: { 'logLevel': 'debug', 'theme': 'neutral' } }%%
      gitGraph
        commit
        branch hotfix
        checkout hotfix
        commit
        branch develop
        checkout develop
        commit id:"ash" tag:"abc"
        branch featureB
        checkout featureB
        commit type:HIGHLIGHT
        checkout main
        checkout hotfix
        commit type:NORMAL
        checkout develop
        commit type:REVERSE
        checkout featureB
        commit
        checkout main
        merge hotfix
        checkout featureB
        commit
        checkout develop
        branch featureA
        commit
        checkout develop
        merge hotfix
        checkout featureA
        commit
        checkout featureB
        commit
        checkout develop
        merge featureA
        branch release
        checkout release
        commit
        checkout main
        commit
        checkout release
        merge main
        checkout develop
        merge release
{{</mermaid>}}
```