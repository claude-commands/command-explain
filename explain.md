---
argument-hint: "<file|function|directory>"
description: "Deep-dive explanation of code with diagrams"
model: claude-opus-4-5-20251101
allowed-tools: ["Bash", "Read", "Glob", "Grep", "AskUserQuestion"]
---

**If `$ARGUMENTS` is empty or not provided:**

Display usage information and ask for input:

**Usage:** `/explain <target>`

**Examples:**
- `/explain src/auth/login.ts` - Explain a file
- `/explain handleAuthentication` - Explain a function
- `/explain src/services/` - Explain a module/directory

**Workflow:**
1. Analyze the specified code target
2. Extract key concepts, dependencies, relationships
3. Generate documentation-style explanation
4. Create Mermaid diagrams for complex flows
5. Identify potential improvements

Ask the user: "What file, function, or directory would you like me to explain?"

---

**If `$ARGUMENTS` is provided:**

Generate a comprehensive explanation of the specified code. Creates documentation-style output with Mermaid diagrams for complex flows.

## Configuration

- **Target**: `$ARGUMENTS` (file path, function name, or directory)

## Steps

1. **Identify Target Scope**
   - If file path: Read the entire file
   - If function name: Search for the function across the codebase
   - If directory: Analyze the module/package structure

2. **Analyze Code Structure**

   For **files**, extract:
   - Purpose and responsibility (what problem does this solve?)
   - Exports/public interface
   - Dependencies (imports)
   - Key functions/classes/types
   - Configuration or constants

   For **functions**, extract:
   - Signature and parameters
   - Return type/value
   - Side effects
   - Error handling
   - Callers and callees

   For **directories**, extract:
   - Module purpose and boundaries
   - File organization pattern
   - Entry points
   - Internal vs external dependencies

3. **Generate Explanation**

   Structure the output as:

   ```markdown
   # [Target Name]

   ## Overview
   [2-3 sentence summary of what this code does and why it exists]

   ## Key Concepts
   - **[Concept 1]**: Brief explanation
   - **[Concept 2]**: Brief explanation

   ## How It Works
   [Step-by-step explanation of the main flow]

   ## Dependencies
   - `module-a`: Used for X
   - `module-b`: Provides Y

   ## Diagram
   [Mermaid diagram - see step 4]

   ## Important Details
   - [Edge cases handled]
   - [Performance considerations]
   - [Security notes]

   ## Potential Improvements
   - [Optional: any obvious improvements or concerns]
   ```

4. **Create Mermaid Diagrams**

   Choose the appropriate diagram type:

   **Flowchart** - For control flow, algorithms, decision trees:
   ```mermaid
   flowchart TD
       A[Start] --> B{Condition}
       B -->|Yes| C[Action 1]
       B -->|No| D[Action 2]
       C --> E[End]
       D --> E
   ```

   **Sequence Diagram** - For interactions between components:
   ```mermaid
   sequenceDiagram
       participant Client
       participant Server
       participant Database
       Client->>Server: Request
       Server->>Database: Query
       Database-->>Server: Results
       Server-->>Client: Response
   ```

   **Class Diagram** - For OOP structures and relationships:
   ```mermaid
   classDiagram
       class User {
           +string name
           +string email
           +login()
           +logout()
       }
       User --> Session
   ```

   **State Diagram** - For state machines:
   ```mermaid
   stateDiagram-v2
       [*] --> Idle
       Idle --> Processing: start
       Processing --> Complete: success
       Processing --> Error: failure
   ```

5. **Adjust Depth Based on Complexity**
   - Simple function (< 20 lines): Brief explanation, maybe no diagram
   - Medium complexity: Full explanation with 1 diagram
   - Complex system: Layered explanation with multiple diagrams
   - Start high-level, offer to drill into specific parts

6. **Add Context**
   - Link to related files/functions
   - Reference any tests that demonstrate usage
   - Note any TODO comments or known issues in the code
   - Mention relevant documentation if it exists

## Output Guidelines

- Write for someone unfamiliar with this specific code
- Avoid restating the code verbatim - explain the "why" and "how"
- Use the same terminology as the codebase
- Keep diagrams focused (5-10 nodes max per diagram)
- Offer to explain any referenced code in more detail

## Use Cases

- **Onboarding**: Understanding unfamiliar code
- **Code archaeology**: Figuring out legacy systems
- **Documentation**: Generating docs from code
- **Review prep**: Understanding code before reviewing
- **Debugging**: Tracing complex flows
