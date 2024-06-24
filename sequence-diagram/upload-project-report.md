# Sequence Diagram: Upload Project Report

```mermaid

    sequenceDiagram

        box Client
        actor Viewer
        actor Writer
        participant UI
        end

        box Server
        participant ProjectController
        participant ProjectService
        participant OutputService
        participant EventGateway
        end

        participant Database

        Writer->>UI: click upload button
        activate UI
        UI-->>Writer: show choose file dialog
        Writer->>UI: submit file
        UI->>ProjectController: POST /project/{projectId}/report/add 
        activate ProjectController
        ProjectController->>ProjectService: uploadProjectFile()
        activate ProjectService
        ProjectService->>Database: checkIfProjectActive()
        activate Database
        Database-->>ProjectService: is project active
        deactivate Database
        alt subproject is active
        ProjectService->>Database: projectFile.create()
        activate Database
        Database-->>ProjectService: saved report
        deactivate Database
        
        ProjectService->>Database: projectLog.create()
        activate Database
        Database-->>ProjectService: project log
        deactivate Database
        
        ProjectService->>OutputService: projectLog()
        activate OutputService
        OutputService->>EventGateway: emit project log 
        activate EventGateway
        deactivate OutputService
        EventGateway-->>UI: broadcast project new report log
        deactivate EventGateway
        UI-->>Viewer: add project log

        ProjectService->>OutputService: projectFile()
        activate OutputService
        OutputService->>EventGateway: emit project report 
        activate EventGateway
        deactivate OutputService
        EventGateway-->>UI: broadcast project new report
        deactivate EventGateway
        UI-->>Viewer: add project report

        else subproject not active
        ProjectService-->>ProjectController: throw notFound
        ProjectController-->>UI: respose error
        UI-->>Writer: show upload project failed notification  
        end
        deactivate ProjectService
        deactivate ProjectController
        deactivate UI
```
