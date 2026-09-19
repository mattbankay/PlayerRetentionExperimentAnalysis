# PlayerRetentionExperimentAnalysis

# Player Retention Experiment Analysis

---

## Data can be found at: https://www.kaggle.com/datasets/yufengsui/mobile-games-ab-testing

## Overview

This analysis evaluates an A/B test for **Cookie Cats**, a mobile puzzle game. The experiment tested whether moving the first progression gate from level 30 to level 40 would improve player retention and engagement.

The central business question is straightforward: **should the product team replace the existing level-30 gate with the proposed level-40 gate?**

The results do not support the change. Seven-day retention declined under gate 40, while one-day retention and game-round activity provided no evidence of an offsetting benefit.

The recommended decision is to **keep the gate at level 30**. Before formally closing the experiment, the product team should confirm the intended allocation ratio because the public dataset does not include the original experiment configuration.

---

## Business Objectives

* Determine whether moving the progression gate improves player retention
* Measure the size and uncertainty of the treatment effect
* Check whether engagement changed enough to alter the retention decision
* Confirm that the experiment data are reliable enough to support a rollout decision

---

## Key Business Questions

1. Did the level-40 gate improve seven-day retention?
2. What happened to one-day retention and game-round engagement?
3. Do the available data reveal any experiment-quality concerns?
4. Should the company ship the change, keep the current design, or rerun the test?

---

## Executive Summary

The experiment included **90,189 players**, with 44,700 assigned to the existing level-30 gate and 45,489 assigned to the proposed level-40 gate.

Seven-day retention declined from **19.02% to 18.20%**, a reduction of **0.82 percentage points**. The 95% confidence interval ranges from a decline of **1.33 points to 0.31 points**, and the result is statistically significant (p=0.0016).

At a scale of 100,000 new players, the point estimate represents approximately **820 fewer players returning after seven days**. The plausible loss ranges from roughly **310 to 1,330 players per 100,000 installs**.

One-day retention also moved lower, declining by **0.59 percentage points**, although that result was not statistically significant on its own. Player engagement did not improve: the median player completed 17 rounds under gate 30 and 16 under gate 40, while the winsorized mean comparison found no meaningful difference between groups.

The business decision is to **keep gate 30 and not ship gate 40**. A new design should demonstrate an improvement before replacing the current experience. Gate 40 did not meet that standard: it reduced the primary metric and produced no compensating engagement benefit.

That recommendation assumes the documented assignment is valid. If a review of the original configuration reveals a broken assignment or exposure process, the result should not be used for a permanent decision and the experiment should be rerun.

---

## Headline Results

| Metric              | Gate 30 | Gate 40 |   Difference | 95% Confidence Interval | Result                            |
| ------------------- | ------: | ------: | -----------: | ----------------------: | --------------------------------- |
| One-day retention   |  44.82% |  44.23% |     −0.59 pp |       [−1.24, +0.06] pp | Directionally lower; inconclusive |
| Seven-day retention |  19.02% |  18.20% | **−0.82 pp** |   **[−1.33, −0.31] pp** | Statistically significant decline |
| Median game rounds  |      17 |      16 |     −1 round |      Exploratory metric | No engagement improvement         |

![Retention treatment effects](assets/retention_effects.png)

### Scale of the Primary Result

* **Players analyzed:** 90,189
* **Estimated seven-day retention loss:** 820 players per 100,000 installs
* **Plausible loss range:** 310 to 1,330 players per 100,000 installs
* **Relative decline in seven-day retention:** 4.3%
* **Statistical result:** p=0.0016

---

## Retention Findings

### Gate 40 Reduced Seven-Day Retention

Seven-day retention is the primary outcome because it better reflects whether players remain engaged beyond their initial experience.

Retention fell from **19.02% for gate 30 to 18.20% for gate 40**. The confidence interval is entirely below zero, indicating that the lower retention rate is unlikely to be explained by normal sampling variation.

The data do not show why the earlier gate performs better. One possible explanation is that the level-30 gate creates a more effective break, reward cycle, or pacing point that encourages players to return. That product explanation would need to be tested separately.

### One-Day Retention Also Moved Lower

One-day retention declined from **44.82% to 44.23%**.

The confidence interval narrowly includes zero, so this result is not conclusive by itself. However, it does not provide support for the new design and is directionally consistent with the seven-day result.

---

## Engagement Findings

Game-round activity is highly skewed. One gate-30 player recorded 49,854 rounds, which makes the unadjusted average misleading.

The analysis therefore emphasizes medians, percentiles, a rank-based comparison, and a winsorized sensitivity check that limits the influence of extreme observations.

* **Median rounds:** 17 for gate 30 and 16 for gate 40
* **Winsorized difference:** −0.26 rounds
* **95% confidence interval:** −1.43 to +0.91 rounds
* **Rank-biserial effect:** −0.0075

There is no meaningful evidence that moving the gate to level 40 increased gameplay enough to justify the retention loss.

---

## Experiment Quality Checks

### Assignment Balance

Gate 40 contains 50.44% of the sample. This split would be unusual if the intended assignment ratio were exactly 50/50 (p=0.0087), but the planned ratio is not included in the dataset.

The difference should therefore prompt a review of the experiment configuration; by itself, it does not prove that randomization failed.

### Allocation Stability

Treatment share is stable across player-ID deciles (p=0.605). Player ID is not a documented timestamp, so this is only a limited stability check and cannot confirm that assignment remained stable over calendar time.

### Data Validation

The notebook confirms:

* 90,189 rows and 90,189 unique players
* no duplicate player IDs
* no missing values
* valid treatment labels and binary retention fields
* no negative game-round values
* a matching SHA-256 checksum for the source file

---

## Business Implications

### 1. Shipping Gate 40 Would Put Retention at Risk

The proposed design produces fewer returning players without a measurable engagement benefit. At scale, that loss would affect the value generated from acquisition spending and reduce the population available for future play and monetization.

### 2. Product Decisions Should Be Based on Incremental Impact

The decision is not based only on which group has the higher retention rate. The randomized experiment estimates the incremental impact of assigning players to gate 40 rather than gate 30, subject to the data-quality limitations described above.

### 3. Experiment Configuration Is Part of the Decision

A statistically significant result does not remove the need to verify how players entered the experiment. The assignment ratio and exposure process should be confirmed before the result is archived as final.

### 4. The Next Test Should Target a Different Mechanism

The results do not justify further investment in the same gate-40 design. The next experiment should test a different hypothesis about player progression, rewards, difficulty, or gate messaging.

---

## Recommendations

### Keep the Existing Level-30 Gate

Do not roll out gate 40. The treatment did not demonstrate the improvement required to replace the current design, and the primary metric instead showed a statistically significant decline. The decision does not depend on proving that the loss exceeds the illustrative 0.5-point threshold.

### Verify the Experiment Configuration

Confirm the intended assignment ratio, assignment timing, exposure rules, and whether all assigned players are included in the dataset. If that review identifies a material implementation problem, rerun the experiment before making a permanent product decision.

### Quantify the Financial Impact

Connect the retention effect to acquisition volume, player lifetime value, advertising revenue, and in-app purchases. This would translate the loss of retained players into a direct revenue estimate.

### Test a Different Progression Strategy

Evaluate alternatives such as reward timing, difficulty pacing, gate messaging, or the cost of continuing rather than simply delaying the same gate.

### Improve Future Experiment Data

Include experiment timestamps, exposure status, pre-treatment player attributes, acquisition source, device information, and monetization outcomes in future extracts.

---

## Analysis Approach

Players were analyzed according to their assigned gate. Seven-day retention was treated as the primary metric, one-day retention as a secondary metric, and game rounds as exploratory.

The decision standard is intentionally conservative: gate 40 must demonstrate a retention improvement to justify replacing the existing gate. An inconclusive result would not support rollout, while credible evidence of lower retention supports keeping gate 30.

The analysis includes:

* data-quality and duplicate-player checks
* sample-ratio and allocation-stability checks
* difference-in-proportions hypothesis tests
* absolute and relative treatment effects
* 95% confidence intervals
* impact estimates per 100,000 players
* approximate minimum detectable effect calculations
* rank-based and winsorized engagement comparisons

Game rounds were not used to define player segments because they occur after assignment and may themselves be affected by the treatment.

---

## Tools Used

* **Python** for data preparation and experiment analysis
* **pandas and NumPy** for data validation and calculation
* **SciPy** for hypothesis testing and confidence intervals
* **Matplotlib and seaborn** for data visualization
* **Jupyter Notebook** for reproducible analysis and reporting

---

## Data and Limitations

The public data originate from the [Kaggle Mobile Games A/B Testing dataset](https://www.kaggle.com/datasets/yufengsui/mobile-games-ab-testing). The notebook retrieves a fixed public mirror and verifies the file against this SHA-256 digest:

```text
9f53027065840672e77303281289988371d4a6b67c7dcd3bd4e6306a2a263dc8
```

The dataset does not include the original experiment plan, experiment dates, intended allocation ratio, exposure rules, detailed metric definitions, acquisition attributes, or monetization outcomes. The analysis can estimate the effect on retention but cannot independently audit the production experiment or calculate direct revenue impact.

The 0.5-percentage-point practical threshold used in the notebook is an analyst assumption, equivalent to 500 retained players per 100,000 installs. A product team would normally define that threshold using business economics and risk tolerance before the experiment begins.

---



Select **Restart Kernel and Run All Cells**. The notebook downloads the source CSV into `data/` when needed, verifies its checksum, and regenerates the treatment-effect chart.
