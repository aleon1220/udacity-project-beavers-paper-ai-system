# udacity-project-beavers-paper-ai-system

udacity project multi-agent system for The Beaver's Choice Paper Company

## Agent Workflow Diagram

High level design

[Mermaid](https://mermaid.live/)

```mermaid
graph TD
    %% Entities
    Customer[Customer Request / CSV Input]
    Database[(SQLite Database)]
    
    %% Agents
    Orchestrator[Orchestrator Agent]
    Inventory["Inventory Agent"]
    Quoting["Quoting Agent"]
    Sales["Sales Agent"]

    %% Flow
    Customer -->|Inquiry/Order| Orchestrator
    Orchestrator <-->|stock interaction| Inventory
    Orchestrator <-->|Request price quote| Quoting
    Orchestrator <-->|Confirm transaction| Sales
        
    Orchestrator -->|Transparent output + Rationale | Customer

    %% Tools mapped to starter code functions
    Inventory -.->| use python tools inventory | Database
    Quoting -.->|use python tools quote | Database
    Sales -.->| use python tools transaction | Database
```

## Agent implementation details

Comprehensive Tool Mapping to all agents and tools

```mermaid
graph TD
    %% Entities
    Customer[Customer Request / CSV Input]
    DB[(SQLite Database)]
    
    %% Agents
    Orchestrator[Orchestrator Agent<br/>Delegates tasks & talks to customer]
    Inventory[Inventory Agent<br/>Manages stock & reordering]
    Quoting[Quoting Agent<br/>Analyzes history & generates pricing]
    Sales[Sales Agent<br/>Finalizes orders & delivery]

    %% Flow
    Customer -->|Inquiry/Order| Orchestrator
    Orchestrator -->|Ask stock/reorder| Inventory
    Orchestrator -->|Request price quote| Quoting
    Orchestrator -->|Confirm transaction| Sales
    
    Inventory -->|Return stock status| Orchestrator
    Quoting -->|Return quote details| Orchestrator
    Sales -->|Confirm| Orchestrator
    Orchestrator -->|Transparent output + Rationale| Customer

    %% Tools mapped to actual starter code functions
    Inventory -.->|"Tool: check_inventory<br/>uses: get_all_inventory, get_stock_level<br/><br/>Tool: check_delivery<br/>uses: get_supplier_delivery_date"| DB
    Quoting -.->|"Tool: check_history<br/>uses: search_quote_history"| DB
    Sales -.->|"Tool: process_order<br/>uses: create_transaction<br/><br/>Tool: financial_audit<br/>uses: get_cash_balance, generate_financial_report"| DB
```
