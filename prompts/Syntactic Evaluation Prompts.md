# Syntactic Evaluation Prompt

This prompt is used to evaluate the syntactic quality of UML class diagrams.
It corresponds to the syntactic dimension defined in the base study, with
error categories matching those used in expert assessments.

The `{PlantUML}` placeholder is replaced at runtime with the PlantUML code
of the diagram under evaluation.

## Prompt

You are a software modelling expert. Your task is to evaluate the <Syntactic Quality> of the UML class diagram provided below, written in PlantUML syntax.

Check only the following rules:

1. Missing cardinality details
2. Class names that violate identifier rules (e.g., contain invalid characters, are missing, or malformed). Association names are optional but, if present, must be syntactically valid. Differences in naming style (CamelCase, underscores, capitalization, spelling, or word choice) must NOT be treated as errors.
3. Incorrect or invalid UML symbols or PlantUML syntax that would prevent correct rendering

Do not evaluate any other aspects such as semantics or readability.

### Instructions

1. Parse the PlantUML diagram.
2. Check each rule systematically.
3. For each detected violation, explain:
   - Which rule was violated,
   - Which element is affected,
   - Your reasoning.
4. Provide the total number of syntactic errors.

### Output format

Output valid JSON only, following this structure:

​```json
{
  "error_count": "<integer>",
  "errors": [
    {
      "rule": "<one of: Missing cardinality details / Inappropriate naming / Incorrect UML symbol>",
      "element": "<class or association name, if available>",
      "description": "<short explanation>"
    }
  ]
}
​```

### Input

UML class diagram in PlantUML syntax: `{PlantUML}`
