# SupervisedLearning
Assignments from the JKU course on supervised learning
Workflow:
```mermaid
sequenceDiagram
    participant User
    participant CLI as "main.py"
    participant AgentCore
    participant ReasoningEngine
    participant LLM
    participant Tools as "e.g., DataAnalyzer"
    participant LTM as "LongTermMemory"

    User->>CLI: Enters task (e.g., "Report for May 2023")
    CLI->>AgentCore: agent.run(task)
    AgentCore->>ReasoningEngine: decide_next_action(context)
    ReasoningEngine->>LLM: Generates prompt, queries LLM
    LLM-->>ReasoningEngine: Returns reasoning + action JSON
    ReasoningEngine-->>AgentCore: Parses action (e.g., use data_analyzer_report)
    AgentCore->>Tools: execute_tool(data_analyzer_report, start='2023-05-01', end='2023-05-31')
    Tools->>LTM: get_expenses(dates)
    LTM-->>Tools: Returns expense DataFrame
    Tools-->>AgentCore: Processes data, returns report dict (with status)
    AgentCore->>ReasoningEngine: decide_next_action(updated_context)
    ReasoningEngine->>LLM: Queries LLM (sees report done, decides to finish)
    LLM-->>ReasoningEngine: Returns reasoning + finish action
    ReasoningEngine-->>AgentCore: Parses finish action
    AgentCore->>ReasoningEngine: generate_task_summary(context)
    ReasoningEngine->>LLM: Queries LLM for summary
    LLM-->>ReasoningEngine: Returns summary text
    ReasoningEngine-->>AgentCore: Returns summary
    AgentCore-->>CLI: Returns final result (status, summary, tool output)
    CLI->>User: Prints formatted result
