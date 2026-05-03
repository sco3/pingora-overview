```mermaid
graph TD
    %% Define Nodes
    Start((Client Request))
    
    subgraph Phase_1 [Phase 1: Setup]
        Init(fn new_ctx - Initialize Request Context)
    end

    subgraph Phase_2 [Phase_2: Pre-Processing REQUEST]
        EarlyFilter(fn early_request_filter - Rate Limiting)
        ReqFilter(fn request_filter - Modify Headers)
        Upstream(fn upstream_peer - Routing Decision)
    end

    subgraph Phase_3 [Phase 3: Upstream Connection]
        Connect(Connect to Origin)
        Origin[(Origin Server)]
        Receive(Receive Response)
    end

    subgraph Phase_4 [Phase 4: Post-Processing RESPONSE]
        ResFilter(fn response_filter - Modify Headers)
        BodyFilter(fn response_body_filter - Modify Body)
    end

    subgraph Phase_5 [Phase 5: Finalization]
        EndRes((Response to Client))
        Summary(fn request_summary - Logging/Metrics)
    end

    %% Flow logic
    Start --> Init
    Init --> EarlyFilter
    EarlyFilter --> ReqFilter
    ReqFilter --> Upstream
    
    Upstream --> Connect
    Connect <--> Origin
    Connect --> Receive
    
    Receive --> ResFilter
    ResFilter --> BodyFilter
    
    BodyFilter --> EndRes
    BodyFilter -.-> Summary```
