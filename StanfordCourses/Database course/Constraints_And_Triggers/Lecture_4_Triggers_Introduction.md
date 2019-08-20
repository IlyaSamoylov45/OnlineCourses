Triggers
  - "Event-Condition-Action Rules"
  - When event occurs, check condition; if true do action.
    - Move monitoring logic from apps into DBMS
    - Enforce constraints
      - Beyond what constraint system supports
      - Automatic constraint "repair"
    - Implementations vary significantly

Triggers in SQL
  - Trigger Syntax
  ```
  Create Trigger name
  Before | After | Instead of events -> Insert on T, Delete on T, Update on T
  [referencing-variables] -> old row as var, new row as var, new table as var, old table as var
  [For Each Row] -> Once for each modified tuple
  when (condition)
  action
  ```

Tricky Issues
  - Row level vs statement level
    - new/old row and new/old table
    - Before, Instead Of
  - Multiple triggers activated at same time
  - Trigger actions activating other triggers (chaining)
    - Also self-triggering, cycles, nested invocations
  - Conditions in when vs as part of action
  - Implementations vary significantly
