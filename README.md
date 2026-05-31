# Mascot-Management-ESIC-Automation-Ui-Path-Studio
A lean UiPath bot for ESIC portal registration. It runs sequentially in a single browser session without heavy retry scopes.  Smart Validation: Uses Check App State to spot existing PANs.  Excel Logging: Marks duplicates as "Already Registered" and skips ahead.  File Sync: Downloads and saves ID cards directly into employee folders.

## Visual Workflow Architecture

```mermaid
graph TD
    A[Start: Read Excel Data] --> B[Open ESIC Portal & Login]
    B --> C[For Each Row / Employee]
    
    subgraph Loop Iteration
        C --> D[Attach Browser Session]
        D --> E[Navigate to Registration Form]
        E --> F[Type Name & Core Details]
        F --> G[Type PAN Number]
        G --> H{Check App State:<br>Already Registered?}
        
        %% Error Branch
        H -- Yes / Warning Appears --> I[Close Window/Tab]
        I --> J[Write Cell: 'Already Registered']
        J --> K[Continue Activity]
        K --> C
        
        %% Success Branch
        H -- No / Clear Path --> L[Upload Photo & Submit]
        L --> M[Download ESIC ID Card]
        M --> N[Rename & Move PDF to Employee Folder]
        N --> O[Close Window/Tab]
        O --> C
    end
    
    C --> P[End: Save Excel Tracker & Close Browser]
