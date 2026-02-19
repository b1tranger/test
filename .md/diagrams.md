```mermaid
useCaseDiagram
    actor "Staff Member" as Staff
    actor "Department Head" as Head
    actor "Logistics Manager" as Manager
    actor "Technician" as Tech

    package "LMRS System" {
        usecase "Submit Requisition" as UC1
        usecase "Report Equipment Issue" as UC2
        usecase "Approve/Reject Request" as UC3
        usecase "Assign Maintenance Task" as UC4
        usecase "Update Asset Inventory" as UC5
        usecase "Generate Reports" as UC6
    }

    Staff --> UC1
    Staff --> UC2
    Head --> UC3
    Manager --> UC3
    Manager --> UC4
    Manager --> UC5
    Manager --> UC6
    Tech --> UC4
```
```mermaid
classDiagram
    class User {
        +int userID
        +string name
        +string role
        +login()
    }
    class Asset {
        +string assetID
        +string category
        +string status
        +tagQR()
        +trackDepreciation()
    }
    class Requisition {
        +int reqID
        +date requestDate
        +string status
        +submitRequest()
        +updateStatus()
    }
    class MaintenanceTicket {
        +int ticketID
        +string description
        +string priority
        +assignTech()
    }

    User "1" -- "0..*" Requisition : creates
    User "1" -- "0..*" MaintenanceTicket : reports
    Asset "1" -- "0..*" MaintenanceTicket : undergoes
    Requisition "0..*" -- "1" User : approved_by
```

```mermaid
sequenceDiagram
    participant S as Staff
    participant SYS as LMRS System
    participant H as Dept Head
    participant M as Logistics Manager

    S->>SYS: Submit Requisition Request
    SYS->>H: Notify Pending Approval
    H->>SYS: Approve Request
    SYS->>M: Route to Logistics
    M->>SYS: Final Approval & Dispatch
    SYS->>S: Notify Status: "Dispatched"
```

```mermaid
graph LR
    User((User)) -- Issue/Request --> Proc(Process Request/Ticket)
    Proc -- Store Data --> DB[(PostgreSQL Database)]
    DB -- Fetch Data --> Report(Reporting & Analytics)
    Asset((Asset Inventory)) -- QR Scan --> Proc
    Report -- Monthly PDF --> Admin((Admin/Finance))
```
```mermaid
stateDiagram-v2
    [*] --> Available: New Entry/Unique Tagging
    Available --> InUse: Allocated to Lab/Staff
    InUse --> Malfunctioning: Issue Reported
    Malfunctioning --> UnderRepair: Technician Assigned
    UnderRepair --> InUse: Fixed
    InUse --> Depreciated: End of Lifespan
    Depreciated --> [*]
```

