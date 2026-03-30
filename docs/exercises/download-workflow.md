---
title: Download flow
parent: Exercises
nav_order: 4
---

# Download flow

## General download sequence

Both hands-ons and exercises are downloaded using the same `gitmastery download` command.

Hands-ons use the `hp-` prefix before the hands-on name.

```mermaid
flowchart
a[gitmastery download] --> a1[Read exercises_source from .gitmastery.json]
a1 --> b[Check if prefixed with hp-]
b -- prefixed with hp- --> c[Download hands-on]
b -- else --> d[Download exercise]
```

{: .note }

> The source that exercises and hands-ons are fetched from is controlled by `exercises_source` in `.gitmastery.json`. See [`exercises_source`](/developers/docs/app/configuration#exercises_source) for the full details and override options.

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
flowchart TD
A[Check exercise exists] -->|Not found| ERR[Error and stop]
A -->|Found| B[Prepare exercise folder]
B --> C[Download base files and read config]
C --> D{Prerequisites met?\nGit / GitHub}
D -->|No| ROLL[Rollback and stop]
D -->|Yes| E[Download additional resources if any]
E --> F{Repo type managed?}
F -->|No| G[Save config and print next steps]
F -->|Yes| H[Save metadata to .gitmastery-exercise.json]
H --> I{Repo type}
I -->|local| J[Create local repo]
I -->|remote| K{Fork required?}
K -->|Yes| L[Fork and clone]
K -->|No| M[Clone repository]
J & L & M --> N[Run download.py setup]
N --> O[Print next steps]
G --> DONE[Download complete]
O --> DONE
```
