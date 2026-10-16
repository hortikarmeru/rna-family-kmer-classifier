# RNA family prediction from sequence

Can short local sequence patterns alone tell RNA families apart?

**Data:** Rfam 15.1's PDB-to-family mapping joined with all nucleic acid chains in the PDB
(seqres, October 2026). After cleaning (removing DNA, multi-family chains, chains under 80%
family coverage, duplicate sequences, and families with fewer than 20 examples), 1,456
sequences across 11 families remain.

## K-mer random forest ([notebook](RNA_Family_Prediction.ipynb))

**Method:** 4-mer proportions (256 features) → random forest, 80/20 stratified split.
Length-only random forest as a baseline.

**Result:** 0.98 accuracy with k-mers vs. 0.86 with length alone.

**Limitations:** Six of the families are rRNA split by domain of life, and near-identical
sequences can appear on both sides of the random split, so the score is likely optimistic.
See the end of the notebook for details.

## Linear and logistic regression ([notebook](RNA_Regression_Models.ipynb))

Same cleaned data, using sequence length and A/U/C/G fractions as features.

**Linear regression (predicting Rfam bit score):** R² 0.91 from length alone (MAE 233 vs.
1043 for always guessing the average), but only R² 0.36 from base fractions alone.

**Logistic regression (tRNA or not):** base fractions alone never predict tRNA at the default
0.5 threshold, matching the 0.71 lazy guess. A 0.3 threshold gets 0.78 accuracy, and adding
length gets 0.96.

**Takeaway:** Length does most of the work in both models. Base fractions ignore letter order,
which is likely why they do so much worse than k-mers.
