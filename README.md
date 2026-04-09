# udacity-project-beavers-paper-ai-system

udacity project multi-agent system for The Beaver's Choice Paper Company. **Munder Difflin Paper Company Multi-Agent System Project**

multi-agent system that supports core business operations at the fictional paper manufacturing company

## Modular multi-agent system

- **Inventory checks** and restocking decisions
- **Quote generation** for incoming sales inquiries
- **Order fulfillment** including supplier logistics and transactions
- DB contains Historical quote data, Incoming customer requests and company synthetic information.

## getting started

- using [uv](https://docs.astral.sh/uv/#the-pip-interface) but feel free to use `pip` or any other python package manager

### inject secrets to a target directory

- leverage onePassword with service account token Win Powershell

   ```powershell
   $OP_SERVICE_ACCOUNT_TOKEN="token from 1Password.com"
   ```

- leverage onePassword with service account token linux Bash

   ```shell
   export OP_SERVICE_ACCOUNT_TOKEN="token from 1Password.com"
   ```

- inject the secrets. `.gitignore` includes required `.env` records

   ```powershell
   op inject --in-file .env.tpl --out-file .env
   ```

### python dependencies & packages

- Install dependencies

    ```fish
    uv venv
    ```

- prepare the python packages

    ```bash
    uv pip sync requirements.txt
    ```

- Activate with:

    ```bash
    source .venv/bin/activate
    ```

- functionality similar to rye or poetry

    ```bash
    uv add ruff
    ```

- validation

    ```bash
    uv run ruff check
    ```

### Smoke test & general execution

- execute the multi agent system

    ```bash
    uv run beavers_choice_multi_agent_system.py
    ```

## Diagrams Designs

### Agent Workflow Diagram

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

### Agent implementation details

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
    Quoting -.-> |"Tool: check_history<br/>uses: search_quote_history" | DB
    Sales -.->|"Tool: process_order<br/>uses: create_transaction<br/><br/>Tool: financial_audit<br/>uses: get_cash_balance, generate_financial_report" | DB
```

---

## Submission Checklist

Make sure to submit the following files:

1. Your completed `template.py` or `project_starter.py` with all agent logic
2. A **workflow diagram** describing your agent architecture and data flow
3. A `README.txt` or `design_notes.txt` explaining how your system works
4. Outputs from your test run (like `test_results.csv`)

---
