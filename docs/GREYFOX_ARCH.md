# GREYFOX Architecture

## Overview

The GREYFOX architcture is designed to be ...

## Block Diagram 

```mermaid
---
title: GreyFox CPU Core
---
graph TD
    subgraph Frontend
        Decoder["32B Decoder (16 opcodes)"] -->|16 opcodes| Linearize[Linearize & Bin] -->
        |1 opcode| BRANCH --> Decoder
    end

    Linearize --> |5 opcodes| FSchedule 
    Linearize --> |6 opcodes| ISchedule
    subgraph Integer
        ISchedule --> INT0 & INT1 & INT2 & INT3 & ISFU --> IREG
        ISchedule --> |2 opcodes|ILDST -->IREG
    end
    subgraph Float
        FSchedule -->  FALU0 & FALU1 & FMUL0 & FMUL1 & FSFU -->FREG
        FSchedule -->  |2 opcodes| FLDST -->FREG
    end
    subgraph Core Cache
        D$<-->TLB
    end
    TLB-->MMU

    INT0 & INT1 & INT2 & INT3 & FALU0 & FALU1 --->COND
    BRANCH <--> COND
    ILDST ------> |2x8B| D$
    FLDST ------> |2x8B| D$

    D$ --> |64B/2clk| Decoder
    subgraph SoC
        D$ <--> |64B/2clk|L2 <--> Memory
        MMU<-->L2
    end
```

