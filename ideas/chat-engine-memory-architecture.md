# Projektidee: Chat Engine mit Memory-Architektur

Notiert am 2026-08-22. Rohe Idee, noch keine Implementierung — dient als
Ausgangspunkt, wenn ich später darauf zurückkomme.

## Architektur-Skizze

```
                         USER
                          │
                          ↓
                   ┌─────────────┐
                   │ Chat Engine │
                   └──────┬──────┘
                          │
             ┌────────────┼────────────┐
             ↓            ↓            ↓
         Working      Retrieval     Planning
          Memory        Engine         │
             │            │            │
             │       ┌────┴────┐       │
             │       ↓         ↓       │
             │   Vector DB  Reranker   │
             │       │         │       │
             └───────┴────┬────┴───────┘
                          ↓
                    Context Builder
                          │
                          ↓
                       Chat LLM
                          │
                          ↓
                       Response
                          │
                          ↓
                  Memory Extractor
                          │
                ┌─────────┼─────────┐
                ↓         ↓         ↓
              Facts    Episodes   Preferences
                │         │         │
                └─────────┼─────────┘
                          ↓
                  Memory Database
                          │
                          ↓
                  Memory Consolidator
                          │
                          └────→ alte Memories
                                  zusammenführen,
                                  aktualisieren,
                                  vergessen
```

## Kernablauf

1. **Eingang:** User-Nachricht geht an die Chat Engine.
2. **Kontextbeschaffung (parallel):** Working Memory (laufender Dialog),
   Retrieval Engine (Vector DB + Reranker) und Planning.
3. **Context Builder** führt die drei Stränge zu einem Prompt zusammen.
4. **Chat LLM** erzeugt die Response an den User.
5. **Memory Extractor** zieht aus dem Verlauf strukturierte Erinnerungen:
   Facts, Episodes, Preferences.
6. **Memory Database** persistiert sie; der **Memory Consolidator** führt
   alte Memories zusammen, aktualisiert sie und vergisst Veraltetes.
