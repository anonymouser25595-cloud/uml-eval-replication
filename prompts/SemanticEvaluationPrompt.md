# Semantic Evaluation Prompt

This prompt is used to evaluate the semantic quality of UML class diagrams.
It corresponds to the semantic dimension defined in the base study, focusing
on the accuracy and completeness of the domain representation.

Unlike the syntactic prompt, this one takes two inputs: the natural-language
problem description and the PlantUML diagram. The placeholders `{PrjDesc}`
and `{PlantUML}` are replaced at runtime with the corresponding content
from `DATASET.csv`.

## Prompt

You are a software modeling expert. Your task is to evaluate the <Semantic Quality> of the UML class diagram provided below, written in PlantUML syntax.

Semantic quality evaluates the accuracy and completeness of the diagram in representing the intended domain. It is measured in terms of:

- **Validity**: all elements and relationships must correctly represent the domain.
- **Completeness**: all necessary elements and relationships must be present.

You must check ONLY the following SEMANTIC rules:

1. Incorrect cardinality
2. Aggregation in place of association
3. Wrong location of attributes or operations
4. Operations cannot be realized using existing attributes and relationships

**IMPORTANT — DO NOT treat the following as semantic errors:**

- Missing association names
- Misspelling or not-proper naming of classes, attributes, or associations
- Extra elements (classes, attributes, operations, associations) that are still logically consistent with the problem description

Do NOT evaluate syntax, notation correctness, readability, or naming quality. Those belong to syntactic or pragmatic quality, not semantic quality.

---

### Instructions

1. Read the problem description to understand the intended domain.
2. Parse the PlantUML diagram.
3. Check each rule systematically.
4. For each detected violation, explain:
   - Which rule was violated,
   - Which element is affected,
   - Your reasoning.
5. Provide the total number of semantic errors.

### Output format

Output valid JSON only, following this structure:

​```json
{
  "error_count": <integer>,
  "errors": [
    {
      "rule": "<one of: Incorrect cardinality / Aggregation in place of association / Wrong location of attributes or operations / Unrealizable operation>",
      "element": "<class or association name, if available>",
      "description": "<short explanation>"
    }
  ]
}
​```

### Input

1. Natural language problem description: `{PrjDesc}`
2. UML class diagram in PlantUML syntax: `{PlantUML}`
