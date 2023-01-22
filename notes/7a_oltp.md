# Online transaction processing - OLTP

## Traditional Stack
Section | What is there
---|---
Application
---|---
SQL | Compiler, Optimizer, Query Processing
Transaction | Transaction manager, locking, undo manager
Caching | Buffer Cache
Logging | Redo logging, crash recovery, backup
Storage | Durable Storage