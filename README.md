# RNA family prediction from k-mer features

Can short local sequence patterns alone tell RNA families apart?

**Data:** Rfam 15.1's PDB-to-family mapping joined with all nucleic acid chains in the PDB
(seqres, October 2026). After cleaning (removing DNA, multi-family chains, chains under 80%
family coverage, duplicate sequences, and families with fewer than 20 examples), 1,456
sequences across 11 families remain.

**Method:** 4-mer proportions (256 features) → random forest, 80/20 stratified split.
Length-only random forest as a baseline.

**Result:** 0.98 accuracy with k-mers vs. 0.86 with length alone.

**Limitations:** Six of the families are rRNA split by domain of life, and near-identical
sequences can appear on both sides of the random split, so the score is likely optimistic.
See the end of the [notebook](RNA_Family_Prediction.ipynb) for details.
