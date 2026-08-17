---
layout: post
title: "Permutation test that failed where it was most needed"
---

## Symmetry
> Thesis: The test statistic in a permutation test **must** be symmetric with respect to a swap of the group labels.

The thesis above leads to a clear conclusion. Our current reporting form, $\frac{M_M - M_K}{M_M}$, is not symmetric: the denominator depends on which group ends up as $M$ after the shuffle. The consequences? Once the labels are reversed, the absolute value of the statistic explodes — a gap of ${90\%}$ becomes $-900\%$ after the groups are swapped. As a result, about half of all permutations count as "extreme", and the p-value converges to a plateau of $\sim{0.31}$ **regardless** of the effect size.

### Power, gaps and a fix
At a gap of 5% the test had a power of 100%, but at a gap of 40% the power dropped to 0% — the test failed in exactly the case it exists for. The fix is a constant denominator, $\frac{M_M - M_K}{M_{pooled}}$: the median of the pooled sample, which is invariant under permutation.
