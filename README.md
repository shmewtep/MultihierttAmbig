# MultiHiertt Ambiguity Annotations Dataset

This dataset extends the [MultiHiertt](https://github.com/psunlpgroup/MultiHiertt) financial question-answering dev set with a manual review layer that flags questions as unambiguous, ambiguous, or unanswerable given the source document.

## Files

| File | Description |
| :--- | :--- |
| `multihiertt_ambiguity_dev.json` | Full MultiHiertt records (`uid`, `paragraphs`, `tables`, `table_description`, `qa`) with one field added: `ambiguity_annotation`. Structurally a drop-in superset of the original MultiHiertt dev.json — code that only reads the original fields will work unmodified. |
| `ambiguity_annotations_only.json` | Just `{uid, ambiguity_annotation}` pairs, for consumers who already hold an official MultiHiertt copy and want to avoid redistributing it. Join on `uid`. |
| `schema.json` | JSON Schema for both files above. |


## Schema

Each record in `multihiertt_ambiguity_dev.json` is a MultiHiertt record plus:

```
ambiguity_annotation:
  status:             'unambiguous' | 'mostly unambiguous' | 'ambiguous' | 'unanswerable'
  subcategory:        'Representational' | 'Pragmatic' | null   # only set when status == 'mostly unambiguous'
  source:             'Data' | 'Query' | 'Both' | null
  description:        string | null   # free-text human explanation of the ambiguity
  taxonomy:
    at_cot:           string[]  # AT-CoT taxonomy labels
    clamber:          string[]  # CLAMBER taxonomy labels
```

`taxonomy` fields are multi-label; a source cell like `"Specify, Semantic"` becomes `["Specify", "Semantic"]`.

## Licensing & Citation

The base questions, documents, and gold answers are from **MultiHiertt** (Zhao et al., 2022, *MultiHiertt: Numerical Reasoning over Multi Hierarchical Tabular and Textual Data*, ACL 2022), distributed under an MIT license.

The `ambiguity_annotation` field (human labels) is original to this project.
