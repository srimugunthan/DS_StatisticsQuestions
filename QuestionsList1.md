### what is a good sample size that you should report your results for LLM summarization task
Evaluating the performance of an LLM on a summarization task is notoriously tricky because there is no single "correct" answer. Unlike classification, where you have a clear ground truth, summarization quality is subjective and multidimensional.

To report results that are scientifically sound and practically useful, your sample size should be determined by the **type of evaluation** you are conducting.

### 1. The "Golden Rule" for Sample Sizes

While there isn’t a one-size-fits-all number, the industry and academic standards generally fall into these buckets:

| Evaluation Type | Recommended Sample Size | Purpose |
| --- | --- | --- |
| **Automated Metrics** | **500 – 1,000+** | For stable ROUGE, BLEU, or BERTScore results across diverse topics. |
| **LLM-as-a-Judge** | **100 – 300** | Balancing cost/latency with statistical significance for semantic grading. |
| **Human Evaluation** | **30 – 100** | The "Gold Standard" for validating nuances, though expensive and slow. |
| **Pilot/Internal Testing** | **20 – 50** | Quick sanity check during prompt engineering or model selection. |

---

### 2. Why these numbers?

Choosing a sample size isn't just about picking a random number; it's about minimizing the **standard error** of your metrics.

* For Automated Metrics (
$$ROUGE$$


, 
$$METEOR$$


): These metrics are highly sensitive to the specific wording of your reference summaries. A small sample (e.g., 20) can lead to a "lucky" high score if the model happens to use the same keywords as the reference. A larger set (
$$N \ge 500$$


) smooths out these lexical coincidences.
* **For LLM-as-a-Judge (e.g., using GPT-4 to grade Llama-3):** Recent research suggests that around **100–200 samples** are sufficient to reach a high correlation with human judgment. Beyond this point, you often see diminishing returns in exchange for significantly higher API costs.
* **For Human Evaluation:** Because of high inter-annotator variance, it is better to have **3 people grade 50 summaries** than **1 person grade 150**. Statistical significance in human trials usually stabilizes around 
$$N=50$$


 if you have high agreement.

---

### 3. Statistical Power and Significance

If you are comparing two models (e.g., Model A vs. Model B), you should aim for a sample size that allows you to calculate a **p-value**.

For summarization, the standard error (

$$SE$$

) of your mean score (like ROUGE-L) is calculated as:

$$SE = \frac{\sigma}{\sqrt{n}}$$

Where:

* $$\sigma$$


 is the standard deviation of scores across your samples.
* $$n$$


 is your sample size.

> **Key Takeaway:** To cut your error margin in half, you need to **quadruple** your sample size.

---

### 4. Diversity Matters More Than Quantity

A sample size of 1,000 news articles is less valuable than a sample of 200 that includes:

* **Varying Lengths:** Short blurbs vs. 10-page reports.
* **Varying Formats:** Transcripts, technical papers, and creative writing.
* **Edge Cases:** Texts with "noise" (typos, HTML tags) or contradictory information.

### Summary Checklist for Reporting

When you write your report, don't just state the mean. Include:

1. The exact 
$$N$$


 count.
2. **Confidence Intervals (95%):** e.g., "ROUGE-L: 
$$0.35 \pm 0.02$$


".
3. **Source of the Data:** (e.g., CNN/DailyMail, XSum, or internal proprietary data).
4. **The Evaluator:** (e.g., "Human experts with 5+ years experience" or "GPT-4o via G-Eval prompt").

==========================
For LLM summarization evaluation, there's no single magic number, but here's how to think about it:

**For research papers and benchmarks**, the common practice is to use 100–1000 samples depending on the dataset and evaluation method. Most published work on summarization (using datasets like CNN/DailyMail, XSum, or SAMSum) reports results on the full test set, which can be thousands of examples. However, when using expensive LLM-as-judge evaluation (like the RAGAS metrics you've been working with), a subset is often necessary.

**Practical guidelines by context:**

For **internal evaluation / iteration cycles**, 50–100 samples is a reasonable starting point. This gives you enough statistical power to detect meaningful differences (e.g., >5% improvement) between two systems while keeping LLM judge costs manageable.

For **formal reporting or publication**, 200–500 samples is a stronger choice. At this range, you can compute confidence intervals that are tight enough to be credible. A common approach is to report mean scores with 95% bootstrap confidence intervals.

For **statistical rigor**, the key consideration is the variance of your metric. If your evaluation scores (say FactualCorrectness or faithfulness) have high variance across samples, you need more data points. You can estimate the required sample size using the formula: n ≈ (z² × σ²) / ε², where ε is your desired margin of error and σ is the standard deviation of your metric scores.

**A few practical tips:**

Stratify your sample — make sure it covers varying input lengths, complexity levels, and domains rather than being randomly drawn, which might skew toward easy cases. Report the standard deviation or confidence intervals alongside mean scores, not just point estimates. If you're comparing two systems (e.g., before/after a prompt change), a paired test on 100+ samples is often sufficient to detect practically meaningful differences.

======
When reporting results for an LLM summarization task, the appropriate sample size depends on the evaluation method—whether you are using **automatic metrics** (e.g., ROUGE, BERTScore) or **human evaluation**. Here’s a breakdown of considerations and typical practices:

---

### 1. Automatic Metrics (e.g., ROUGE, BERTScore, METEOR)

- **Goal**: Obtain stable, reproducible estimates of system performance.
- **Typical Practice**: Use the entire test set of a standard benchmark (e.g., CNN/DailyMail test set has ~10,000 examples, XSum ~11,000). This ensures the results are comparable to prior work and reduces sampling variability.
- **If you create a new dataset**: Aim for at least **500–1,000 examples** to capture diversity in content and length. For fine-grained analysis (e.g., by topic or length), larger samples may be needed.
- **Statistical Consideration**: With thousands of examples, confidence intervals around metric scores become very narrow, making differences of a few ROUGE points meaningful. For smaller test sets (e.g., 100 examples), the margin of error can be large, so always report confidence intervals.

---

### 2. Human Evaluation

Human evaluation is expensive and time-consuming, so sample sizes are smaller but must be chosen carefully to ensure reliability.

- **Common Range**: **50–200 documents/summaries** per system or condition. For example:
  - Comparing two systems: often 100–200 examples.
  - Evaluating a single system: 50–100 examples may suffice for overall quality ratings.
- **Number of Annotators**: Use **3–5 annotators per example** to account for subjective judgments and compute inter-annotator agreement (e.g., Krippendorff’s alpha).
- **Power Analysis**: If you plan to detect a specific difference (e.g., a 0.5-point improvement on a 5-point Likert scale), you can compute the required sample size based on the expected variance, significance level (α = 0.05), and desired power (1-β = 0.80). A rough rule of thumb for pairwise comparisons is **~100 examples** for medium effect sizes.
- **Pilot Studies**: Start with a smaller sample (e.g., 30–50) to estimate variance and refine annotation guidelines, then scale up.

---

### 3. Factors Influencing Sample Size

- **Variability**: High variance in summary quality (e.g., due to diverse input texts) requires larger samples.
- **Effect Size**: Smaller expected differences between systems demand more examples.
- **Number of Systems**: If comparing multiple systems, consider adjustments for multiple comparisons (e.g., Bonferroni correction) or use statistical models like ANOVA.
- **Annotation Cost**: Balance statistical rigor with budget constraints.

---

### 4. Reporting Recommendations

- **For automatic metrics**: Report results on a widely used test set (e.g., CNN/DailyMail test) and include the number of examples. If you create a new test set, describe its size and how it was sampled.
- **For human evaluation**: Clearly state the number of examples, number of annotators, and how they were recruited. Provide confidence intervals or standard errors for mean scores, and report inter-annotator agreement.
- **General Guideline**: In research papers, it’s common to see:
  - Automatic metrics: **1,000–10,000 examples**.
  - Human evaluation: **100–200 examples** with 3–5 judgments each.

---

### 5. Practical Example

Suppose you are evaluating two summarization models (A and B) on a new dataset.  
- **Automatic evaluation**: Use a test set of **1,000 documents** to compute ROUGE scores. Report mean and 95% confidence intervals.  
- **Human evaluation**: Randomly sample **150 documents** from the test set. Have three expert annotators rate each summary on fluency, coherence, and faithfulness using a 5-point scale. Report average scores, confidence intervals, and Fleiss’ kappa.

---

### Summary

| Evaluation Type | Recommended Sample Size | Notes |
|-----------------|-------------------------|-------|
| Automatic Metrics (Standard Benchmark) | Full test set (e.g., 1k–10k) | Ensures comparability and low variance. |
| Automatic Metrics (New Dataset) | ≥500 examples | Larger if high variability. |
| Human Evaluation | 100–200 examples | With 3–5 annotators per example. |
| Pilot Human Evaluation | 30–50 examples | To refine guidelines and estimate variance. |

Always report confidence intervals and, for human evaluation, inter-annotator agreement to demonstrate reliability. The “good” sample size is one that balances statistical power, practical constraints, and the need for reproducible results.

For your RAGAS-based LLM evaluation pipelines specifically, 100–200 samples with bootstrap CIs is a solid, defensible choice for internal reporting, scaling up to 300–500 if you're publishing or presenting to stakeholders.
