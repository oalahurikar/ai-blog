
**Key Data Flows:**

Audio Flow:
- Microphone → Audio Manager → WebSocket Manager → OpenAI
- OpenAI → WebSocket Manager → Audio Manager → Speakers

Event Flow:
- OpenAI → WebSocket Manager → Event Handler → Appropriate Component

Function Call Flow:
- OpenAI → WebSocket Manager → Event Handler → Function Call Handler → Custom Tools
- Custom Tools → Function Call Handler → WebSocket Manager → OpenAI

State Management:
- All components update and read from State Manager
- State Manager ensures consistency across components

```mermaid
graph TB
    subgraph Client Application
        Main[Main Application]
    end

    subgraph RealtimeAPI Core
        WSM[WebSocket Manager]
        EH[Event Handler]
        AM[Audio Manager]
        FCH[Function Call Handler]
        State[State Manager]
    end

    subgraph External Services
        OpenAI[OpenAI WebSocket API]
        Audio[Audio I/O Hardware]
        Tools[Custom Tools/Functions]
    end

    %% Data Flow
    Main -->|Initialize| RealtimeAPI
    RealtimeAPI -->|Manages| WSM
    RealtimeAPI -->|Coordinates| EH
    RealtimeAPI -->|Controls| AM
    RealtimeAPI -->|Delegates| FCH
    RealtimeAPI -->|Monitors| State

    %% WebSocket Flow
    WSM <-->|WebSocket Messages| OpenAI
    
    %% Event Flow
    WSM -->|Events| EH
    EH -->|Updates| State
    EH -->|Triggers| FCH
    EH -->|Controls| AM

    %% Audio Flow
    AM <-->|Audio Stream| Audio
    AM -->|Audio Data| WSM

    %% Function Call Flow
    FCH <-->|Execute Functions| Tools
    FCH -->|Results| WSM

    %% State Updates
    State -.->|State Updates| WSM
    State -.->|State Updates| AM
    State -.->|State Updates| FCH

    classDef core fill:#f9f,stroke:#333,stroke-width:2px
    classDef external fill:#bbf,stroke:#333,stroke-width:2px
    class WSM,EH,AM,FCH,State core
    class OpenAI,Audio,Tools external
```


### Basics of web socket for Python

How web socket is used for audio in Python?