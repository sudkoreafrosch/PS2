# A More Credible Approach to Parallel Trends

**Full citation:** Rambachan, A., & Roth, J. (2023). A more credible approach to parallel trends. Review of Economic Studies, 90(5), 2555–2591. https://doi.org/10.1093/restud/rdad018

---

## 1. What the paper establishes

Difference-in-differences studies defend parallel trends by showing that pre-treatment coefficients are close to zero, but that test can miss violations large enough to distort the result. Rambachan & Roth(2023) replaces the assumption with an explicit bound on how far post-treatment violations may depart from the observed pre-trends, and provides confidence sets valid under that bound. A sensitivity analysis then shows how loose the bound must become before the conclusion fails. In one application it fails once violations may be about twice the largest pre-period violation.

## 2. Checks for a reviewer

**(a) Check whether the manuscript's only defense of parallel trends is an insignificant or visually flat set of pre-period coefficients, while every reported interval is a conventional one.** If so, ask for confidence sets that remain valid under a stated bound on post-treatment violations.

**(b) Check whether the manuscript states the breakdown value, the bound at which the headline hypothesis can no longer be rejected, and interprets that value in substantive units.**

**(c) Check whether the restriction the manuscript imposes matches the confound it names.** Differential shocks call for a bound relative to the pre-period violations. Smoothly evolving secular trends call for a smoothness bound.

**(d) If the manuscript presents group-specific linear trends, or a linear extrapolation of the pre-trend, as its answer to a parallel-trends concern, check whether it also reports results that allow the trend difference to depart from linearity.** A linear adjustment can reverse the sign of an estimate.

**(e) Check whether the sensitivity analysis targets the same parameter as the headline claim.** A robust set for the first post-treatment period does not support a claim about a later-period or average effect, because the set widens with the horizon.

## 3. Evidence

**For (a).** Among the concerns that motivate the paper, the authors list that "pre-trends tests may be underpowered against meaningful violations of parallel trends, potentially leading to severe undercoverage of conventional confidence intervals" (p. 2559). Passing the test does not make the conventional interval trustworthy. When the worst-case bias equals one standard error, a nominal 95% interval covers 83% of the time, and 48% at two standard errors (p. 2580).

**For (b).** The practical guidance states: "It is natural to report both the sensitivity of the researcher's causal conclusion to the choice of this parameter and the "breakdown" parameter value at which particular hypotheses of interest can no longer be rejected" (p. 2583). The value must then be judged against the setting. In the second application the authors convert it into units of teacher value-added and conclude that "the strength with which we can rule out a null effect depends on our assessment of the economic plausibility of such non-linearities" (p. 2587).

**For (c).** The guidance on choosing the restriction begins: "The choice of Δ should be motivated by economic knowledge about the types of possible confounding factors that could produce non-parallel trends." (p. 2582). Here Δ is the set of trend differences the researcher allows. The authors pair differential shocks with relative-magnitude bounds and secular trends with smoothness bounds (p. 2582). In their first application they reject the smoothness class as unreasonable for industry-specific shocks (p. 2584).

**For (d).** The smoothness class has a parameter M that limits how much the slope of the trend difference may change between periods, and "ΔSD(0) requires that the difference in trends be exactly linear, which corresponds with the assumption underlying the parametric linear specification common in applied work" (p. 2564). The linear-trend fix is the zero-deviation corner of a wider class. In the second application the conventional estimate for women is negative, yet the robust set under an exactly linear violation contains only positive values (p. 2587).

**For (e).** Under a bound tied to the largest pre-period violation, the authors conclude that "we will thus typically obtain wider confidence sets for parameters involving later periods" (p. 2585), because the groups have more time to diverge (p. 2584). In the first application the same bound excludes zero for the first-year effect and includes zero for the four-period average (p. 2584).

## 4. Scope

**Applies to:** difference-in-differences and event-study manuscripts that estimate at least one pre-treatment coefficient and whose causal claim depends on rejecting a hypothesis about a post-treatment effect. The framework restricts post-treatment violations given the identified pre-trend (p. 2562), so the checks run on the pre-period coefficients. Check (a) triggers when the pre-trends test or plot is the only defense. Checks (b), (c), and (e) trigger once the manuscript reports these robust sets or claims in words that its result is robust to violations of parallel trends. Check (d) triggers only when a trend-adjusted specification is the remedy and the stated concern is a smoothly evolving trend. Check (e) further requires several post-treatment periods and a restriction that bounds changes across consecutive periods.

**Does not apply to:** the nearby cases where each check should stay silent. Check (a) is silent when robust sets are already reported. It does not license the opposite inference either: in the first application the pre-trends test rejects (p < 0.01), yet the first-year effect survives a bound equal to the largest pre-period violation (pp. 2583–2584). Check (b) must not apply a universal threshold, since a bound of one is a natural benchmark only when the numbers of pre- and post-treatment periods are similar (p. 2563). Check (c) is silent when the manuscript justifies a custom restriction, which the authors allow (p. 2565). Check (d) is silent when the concern is a discrete shock, where a smoothness bound is the wrong class (p. 2584). Check (e) is silent with a single post-treatment period, and the authors limit the widening pattern themselves: it "need not necessarily hold for other types of restrictions" (p. 2584).

**Does not apply to:** the question of whether the chosen restriction is true. The procedures are "uniformly valid so long as the difference in trends satisfies these restrictions" (p. 2588). This is the sharpest boundary in the paper. Passing all five checks shows what the conclusion assumes. It does not show that the assumption holds, and a review that reads a large breakdown value as proof of a causal effect overreaches. The robust sets also assume no treatment effect in the pre-period (p. 2560).

**Partial application:** staggered adoption estimated by a conventional two-way fixed effects regression. The sensitivity analysis can be run on such coefficients, but they have a sensible causal interpretation only if treatment effects are homogeneous across adoption cohorts (p. 2586). Also partial: pre-periods that may be partly treated, where the authors move the reference period back (p. 2586). Flag the concern; do not assert that the analysis applies as it stands.

## 5. What failure looks like

A manuscript shows an event-study figure with conventional 95% bands and writes that the pre-treatment coefficients are jointly insignificant, supporting parallel trends. No appendix relaxes the assumption. The robustness section has a column labeled "group-specific linear trends" and the sentence "results are unchanged when we allow for differential trends," with nothing showing what happens if the trend bends. Where robust sets are reported, they appear at one value of the bound, or the breakdown value is a bare number followed by "the results are robust." The text worries about a recession, but the sensitivity appendix uses the smoothness class without a stated reason. The abstract reports a long-run effect, while the sensitivity figure covers the first post-treatment period only.

## 6. Test cases

**SHOULD trigger:** Lovenheim, Michael F., and Alexander Willén. 2019. "The Long-Run Effects of Teacher Collective Bargaining." American Economic Journal: Economic Policy 11(3): 292–324. The paper uses the staggered passage of state duty-to-bargain laws and estimates event-study coefficients, with pre-period coefficients as the test for selection on trends (pp. 301–302). Its stated primary concern is secular trends that differ with treatment exposure (p. 318). The remedy is the one check (d) targets: a parametric event-study model with a linear pre-treatment trend (p. 315), from which the authors conclude that "our results for men are not biased by pretreatment trends" (p. 316). No result allows the trend to depart from linearity, so checks (a), (b), and (d) should run, with (d) decisive. Checks (c) and (e) stay silent because no restriction-based set is reported. Timing is staggered and the estimates come from a conventional regression, so this is also a partial-application case. The checks ask for a sensitivity analysis and leave the finding itself open.

**Should NOT trigger:** Qin, Marco Shaojun, Xueming Luo, Todd Schifeling, and Yang Wang. 2025. "When Corporate Silence Is Costly: Negative Consumer Responses to Corporate Silence on Social Issues." Strategic Management Journal 46(4): 994–1026. A near-miss: like Lovenheim and Willén, it rests a causal claim on a difference-in-differences estimate, comparing the same firms' Instagram and Twitter accounts four weeks before and after Blackout Tuesday (p. 1008), and its headline result rejects a null (p. 1010). It shows a plot of pre-event trends (p. 1009), but the plot is not its only defense. The authors add an event-study analysis and the Rambachan and Roth sensitivity analysis in Online Appendix 6 (p. 1009), so check (a) stays silent, and that appendix satisfies (b) and (c). Check (d) is silent because no linear-trend adjustment is offered as a remedy. Timing is not staggered, so no partial-application caveat arises. A reviewer can still note that the main text summarizes the appendix as evidence for parallel trends (p. 1009), which a sensitivity analysis cannot supply.
