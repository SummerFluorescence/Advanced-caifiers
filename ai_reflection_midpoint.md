# AI Reflection Midpoint: Phase I Experience

**AI-aided pair:** 
- Siria Yerlany Sadeddin Mejia (Student ID: [K12541583])
- [Theresa Ugochukwu Ukwamedua-Gban] (Student ID: [K12039939])

## 1. Overview of the Phase I Approach
For Phase I, our objective was to investigate the input-output behavior of a ResNet model trained on the ISIC skin lesion dataset, treating the model strictly as a black box. We focused on identifying systematic biases, such as reliance on artefacts (ink, dark corners) and acquisition bias (differences in imaging hardware/labs), without accessing internal gradients or latent embeddings.

## 2. How GenAI Influenced the Analysis
Generative AI significantly accelerated our workflow, particularly in translating theoretical concepts from our lectures into executable Python code. 
- **Methodology Selection:** GenAI helped us map our lecture notes (LIME, SHAP, PDP, UMAP) to the specific constraints of a black-box analysis.
- **Metric Selection:** When we investigated the impact of the lab of origin, GenAI initially suggested standard accuracy. However, through our dialogue, we realized the severe class imbalance in the HAM10000 dataset. GenAI quickly adapted and implemented **Balanced Accuracy** (the mean of per-class recalls), which provided a much more honest evaluation of the model's performance across minority classes like Melanoma (`mel`) and Basal Cell Carcinoma (`bcc`).
- **Hypothesis Generation:** GenAI was instrumental in proposing the "Normalization Hypothesis." After we discovered that the `vidir_molemax` lab formed an isolated cluster in our UMAP pixel-feature space, GenAI suggested applying semantic-preserving normalizations (Z-score, CLAHE, White Patch Retinex) to visually prove which specific environmental cue (e.g., global brightness vs. color cast) was causing the domain shift.

## 3. Strengths and Limitations of GenAI Assistance

### Strengths
- **Rapid Prototyping:** GenAI allowed us to bypass boilerplate coding for complex libraries like `shap`, `lime`, and `umap-learn`, letting us focus on the experimental design and interpretation of the plots.
- **Iterative Refinement:** The tool was highly responsive to corrections. When we pointed out that a proposed perturbation (blurring the whole image center) was too broad, it immediately generated a more sophisticated, targeted approach (blurring only the darkest 5% of pixels based on a percentile threshold).

### Limitations & The Need for Human Oversight
- **Library Version Quirks:** GenAI struggled with specific, version-dependent behaviors of the libraries. For example, it repeatedly failed to handle the 5-dimensional output array `(1, 224, 224, 3, 7)` produced by the SHAP `PartitionExplainer` when plotting with `matplotlib`. It also initially passed NumPy arrays directly to `torchvision` transforms, which expected PIL Images. These errors required manual debugging and a deep understanding of the underlying data structures to fix.
- **Lack of Domain Intuition:** While GenAI can write code to blur an image, it lacks the dermatological context to know *what* to blur. It was human intuition that dictated we should specifically target the "dark crust" texture identified by SHAP, rather than applying a generic Gaussian blur to the center of the image. 
- **Metadata Interpretation:** GenAI initially tried to use `dx_type` (diagnosis method) as a proxy for the lab of origin. It required human inspection of the actual `HAM10000_metadata.csv` file to identify the `dataset` column, which contained the exact acquisition source (e.g., `vidir_modern`, `uq`).

## 4. Conclusion
GenAI acted as a highly capable junior developer and brainstorming partner during Phase I. It excelled at code generation and suggesting statistical approaches, but it required constant human validation to ensure the experiments remained clinically relevant and technically sound. The most valuable insights, such as the targeted blurring of the dark crust and the use of Balanced Accuracy for imbalanced lab data emerged from a collaborative dialogue where human domain knowledge guided the AI's computational capabilities.