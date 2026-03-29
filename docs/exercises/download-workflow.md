---
title: Download workflow
parent: Exercises
nav_order: 4
---

## General download sequence

Both hands-ons and exercises are downloaded using the same `gitmastery download` command.

Hands-ons use the `hp-` prefix before the hands-on name.

```mermaid
flowchart
a[gitmastery download] --> b[Check if prefixed with hp-]
b -- prefixed with hp- --> c[Download hands-on]
b -- else --> d[Download exercise]
```

Refer to the following sections for the specific download sequences.

## Hands-on download

These are handled within the app's `download.py` command through the `download_hands_on` function.

```mermaid
flowchart
a[Download hands-on] --> b[Check that hands-on exists in the hands_on/ folder]
b -- does not exist --> c[Error and stop the download]
b -- else --> d[Check if hands-on already exists]
d -- yes --> e[Delete hands-on and re-download]
e --> f[Create hands-on folder]
d -- no --> f
f --> g[Change directory to hands-on directory]
g --> h[Load hands-on file as namespace to extract variables and functions]
h --> i[Check Git if toggled]
i --> j[Check GitHub if toggled]
j --> k[Execute the download function in the hands-on file]
```

## Exercise download

These are handled within the app's `download.py` command through the `download_exercise` function.

```mermaid
flowchart
A[Check if exercise exists] --> B{Exercise exists?}
B -- No --> C[Error and stop the download]
B -- Yes --> D[Check if exercise folder already exists]
D -- Yes --> E[Delete existing folder]
D -- No --> F[Create exercise folder]
E --> F
F --> G[Change directory to exercise folder]
G --> H[Download base files]
H --> I[Read config file]

I --> J{Requires Git?}
J -- Yes --> K[Check Git setup]
K -- Not setup --> L[Rollback, remove folder, stop download]
J -- No --> M[Proceed]

M --> N{Requires GitHub?}
N -- Yes --> O[Check GitHub setup]
O -- Not setup --> P[Rollback, remove folder, stop download]
N -- No --> Q[Proceed]

Q --> R{Config has base files?}
R -- Yes --> S[Download additional resources]
R -- No --> T[Skip resource download]

S --> U{Repo type is managed?}
T --> U
U -- Yes --> V[Set up exercise folder]
U -- No --> Z1[Save config with timestamp and print next steps]

V --> V1[Save metadata to .gitmastery-exercise.json]
V1 --> V2{Repo type: local or remote?}
V2 -- Local --> V3[Create local folder]
V2 -- Remote --> V4[Retrieve from GitHub]
V4 --> V5{Fork required?}
V5 -- Yes --> V6[Check or delete existing fork, create new fork, clone fork]
V5 -- No --> V7[Clone repository]
V3 --> V8[Change into repo folder]
V6 --> V8
V7 --> V8
V8 --> V9[Fetch resources via download.py]
V9 --> V10{Resources exist?}
V10 -- Yes --> V11[Download and save resources]
V10 -- No --> V12[Skip resource download]
V11 --> V13{Repo init enabled?}
V12 --> V13
V13 -- Yes --> V14[Initialize repo and commit initial state]
V13 -- No --> V15[Skip repo init]
V14 --> V16[Execute setup from download.py]
V15 --> V16
V16 --> V17[Print next steps]

Z1 --> Z2[Download complete]
V17 --> Z2
```
