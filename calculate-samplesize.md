# Indian Electoral Scale: Constituencies and Polling Booths

In India's electoral structure, elections operate at two levels: Parliamentary Constituencies (PC) for the Lok Sabha and Assembly Constituencies (AC) for State Legislative Assemblies.

| Level | Total Units in India | Average Polling Booths per Unit | Total Polling Booths |
|-------|----------------------|--------------------------------|----------------------|
| **Parliamentary Constituency (PC)** | 543 | ≈ 1,900 to 2,000 | ≈ 10,52,664 (2024 General Election) |
| **Assembly Constituency (AC)** | ≈ 4,120 | ≈ 200 to 300 (avg. ≈ 255) | ≈ 10,52,664 |

In a General Election, each Parliamentary Constituency is composed of multiple Assembly Segments (typically 6 to 9 segments depending on the state).

---

## Formulating the Probability Problem

Let an election audit be framed as sampling without replacement from a finite population of voting machines.

- **N** = Total number of polling booths (EVMs) in the population.
- **D** = Number of manipulated or defective booths (p = D/N is the defect rate).
- **n** = Sample size of booths randomly selected for a complete manual VVPAT count.
- **X** = Number of manipulated booths detected in the sample.

Since booths are sampled without replacement, the exact probability distribution of X follows the **Hypergeometric Distribution**:

$$P(X = k) = \frac{\binom{D}{k} \binom{N - D}{n - k}}{\binom{N}{n}}$$

To detect manipulation, the audit requires observing at least one tampered machine (X ≥ 1). The probability of detection (the statistical power/confidence level 1 − α) is:

$$P(\text{Detection}) = P(X \ge 1) = 1 - P(X = 0) = 1 - \frac{\binom{N - D}{n}}{\binom{N}{n}}$$

When N is large relative to n, this is approximated via the **Binomial Distribution**:

$$P(X = 0) \approx \left(1 - \frac{D}{N}\right)^n = (1 - p)^n$$

$$P(\text{Detection}) \approx 1 - (1 - p)^n$$

Setting the desired confidence level to at least 99.99% (1 − α = 0.9999 ⟹ α = 0.0001):

$$(1 - p)^n \le 0.0001$$

$$n \ln(1 - p) \le \ln(0.0001)$$

Since ln(1 − p) < 0, reversing the inequality gives:

$$n \ge \frac{\ln(0.0001)}{\ln(1 - p)} = \frac{-9.21034}{\ln(1 - p)}$$

Using the Taylor approximation ln(1 − p) ≈ −p for small p:

$$n \approx \frac{9.21034}{p}$$

---

## Required Sample Size: Macro vs. Micro Perspectives

The sample size required to reach 99.99% confidence depends on whether the audit targets **systemic national fraud** or **localized margin-of-victory fraud**.

### 1. The National / Systemic Model (The ISI Expert Committee Approach)

In 2019, an Expert Committee from the Indian Statistical Institute (ISI) submitted a report to the Supreme Court evaluating the sample size needed to detect any widespread defect across all EVMs in India (N ≈ 10.35 × 10⁵).

If an adversary implements systemic fraud affecting at least p = 1.9% (roughly 2% of machines, or ≈ 20,000 EVMs nationally):

$$n \ge \frac{\ln(0.0001)}{\ln(1 - 0.01904)} \approx 479$$

The ISI concluded that a random sample of just **479 EVMs** across the entire country would detect at least one fraudulent machine with greater than 99.99% confidence.

### 2. The Localized Constituency Model (Per Assembly Segment)

Elections in India are determined locally by first-past-the-post plurality, not by national popular aggregates. In a single Assembly Constituency with N = 250 booths:

- **Current Supreme Court Mandate:** The Supreme Court mandated counting VVPAT slips from **5** randomly selected booths per Assembly Constituency/Segment.
- For N = 250 and n = 5, the probability of detection P(X ≥ 1) varies by the fraction of booths rigged:

| Tampered Booths (D) | % of Segment (p) | Detection Probability with Current Mandate (n = 5) | Required Sample (n) for 99.99% Confidence |
|---------------------|------------------|-----------------------------------------------------|-------------------------------------------|
| 2 | 0.8% | 3.97% | 247 |
| 5 | 2.0% | 9.68% | 209 |
| 12 | 4.8% | 21.94% | 131 |
| 25 | 10.0% | 41.22% | 74 |
| 50 | 20.0% | 67.56% | 38 |

To be 99.99% confident of catching fraud that affects only 2% of the booths inside a single constituency (D = 5 out of 250), one must audit **209 out of the 250 booths** (≈ 84% of the constituency).

---

## The Audit Reality

- **Nationally:** Across all ≈ 4,120 assembly segments in India, auditing 5 booths per segment yields a total sample size of:

$$\text{Total Sample} = 4,120 \times 5 = 20,600 \text{ booths}$$

This 20,600-booth audit far exceeds the ISI's minimum sample size of 479, providing effectively 100% confidence against any coordinated, multi-state algorithmic manipulation.

- **Locally:** If an adversary attempts surgical, booth-level manipulation in a single constituency with a razor-thin margin, a static sample of 5 booths has a low detection probability. Mitigating that scenario requires **Risk-Limiting Audits (RLAs)**, where the sample size n dynamically scales based on the victory margin rather than remaining a fixed number.
