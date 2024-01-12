# GREYFOX Architecture

## Overview

The GREYFOX architcture is designed to be ...

## Block Diagram 

```mermaid
graph TD
    subgraph Frontend
        Decoder
        Linearize
        BRANCH-->Decoder
    end

    Decoder["32B Decoder (16 opcodes)"] -->|16 opcodes| Linearize[Linearize & Bin]
    Linearize ----> |5 opcodes| FSchedule 
    Linearize --> |6 opcodes| ISchedule
    Linearize ---> |1 opcode| BRANCH
    subgraph Integer
        ISchedule --> INT0 & INT1 & INT2 & INT3 & ILDST & ISFU --> IREG
    end
    subgraph Float
        FSchedule --> FLDST & FALU0 & FALU1 & FMUL0 & FMUL1 & FSFU -->FREG
    end
    subgraph Core Cache
        I$
        D$
        D$<-->TLB
    end
    TLB-->MMU

    
    ILDST & FLDST ---> D$

    I$["I$"] --> |32b cacheline| Decoder
    I$ <--> D$
    subgraph SoC
        D$ <--> L2 <--> Memory
        MMU<-->L2
    end
```

