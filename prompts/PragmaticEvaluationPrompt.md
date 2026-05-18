# Pragmatic Evaluation Prompt

This prompt is used to evaluate the pragmatic quality of UML class diagrams.
It corresponds to the pragmatic dimension defined in the base study, focusing
on diagram clarity, conciseness, and readability rather than structural
correctness or domain accuracy.

Like the semantic prompt, this one takes two inputs: the natural-language
problem description and the PlantUML diagram. The placeholders `{PrjDesc}`
and `{PlantUML}` are replaced at runtime with the corresponding content
from `DATASET.csv`.

## Prompt

You are a software modeling expert. Your task is to evaluate ONLY the <Pragmatic Quality> of the UML class diagram provided below, written in PlantUML syntax.

Check only the following rules:

1. **Redundant attributes and associations**
   - Same attribute appears multiple times in the SAME class without justification.
   - Identical or clearly duplicate associations between the SAME pair of classes (same direction / same role) that don't add new information.

2. **Specialization with no distinction among subclasses**
   - A generalization hierarchy where two or more subclasses:
     - have no extra attributes, AND
     - have no extra operations, AND
     - have no specific associations different from the parent.
   - The subclass names do NOT clearly express distinct roles, categories, or types that are helpful for understanding the domain.

3. **Inconsistency in styling and conventions**
   - Random use of visibility markers, or other notational elements in a way that makes the diagram harder to read.

If you are unsure whether an issue is pragmatic or semantic, you MUST assume it is semantic and IGNORE it.

### Instructions

1. Parse the PlantUML diagram.
2. Check each rule systematically.
3. For each detected violation, explain:
   - Which rule was violated,
   - Which element is affected,
   - Your reasoning.
4. Provide the total number of pragmatic errors.

### Output format

Output valid JSON only, following this structure:

​```json
{
  "error_count": <integer>,
  "errors": [
    {
      "rule": "<one of: Redundant attributes and associations / Specialization with no distinction among subclasses / Inconsistency in styling and conventions>",
      "element": "<class or association name, if available>",
      "description": "<short explanation>"
    }
  ]
}
​```

### Input

1. Natural language problem description: `{PrjDesc}`
2. UML class diagram in PlantUML syntax: `{PlantUML}`
