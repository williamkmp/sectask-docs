# Sequence Diagram: Reject Finding Deletion

```mermaid

    sequenceDiagram

        box Client
        actor Viewer
        actor ProjectManager
        participant UI
        end

        box Server
        participant FindingController
        participant FindingService
        participant OutputService
        participant EventGateway
        participant Database
        end

        ProjectManager->>UI: clicks reject delete button
       
        
        deactivate FindingService
        deactivate FindingController
        deactivate UI
```
