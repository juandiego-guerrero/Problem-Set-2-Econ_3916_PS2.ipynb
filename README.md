# Problem-Set-2-Econ_3916_PS2.ipynb
Problem Set 2

# The Survey That Wasn't Random — Nonresponse Bias in a Satisfaction Survey

**ECON 3916 · Problem Set 2** · Notebook: `Econ_3916_PS2.ipynb`

## Objective
I tested whether a product survey showing a 0.70-point jump in satisfaction after a redesign could be trusted, by building a population where I knew the true answer and checking what the survey, the bootstrap, a t-test and a weighting correction each got right or wrong.

## Methodology
- **Simulated the population (Phase 1).** Built two populations of 50,000 users, before and after the redesign. Satisfaction (1–5) depends on engagement minutes, whether the user filed a support ticket, and random noise. The true improvement was set to about 0.10 points.
- **Imposed a response mechanism.** The chance of answering the survey rises with a user's own satisfaction, rises with engagement and falls with support tickets. After the redesign, the link between satisfaction and responding is much stronger.
- **Measured uncertainty with the bootstrap (Phase 2).** Wrote my own resampling loop (B = 2,000) and compared it with SciPy's percentile and BCa intervals for the mean and median.
- **Ran the PM's test (Phase 3).** Welch two-sample t-test and Cohen's d on the respondents.
- **Ran a power analysis.** Found the sample size needed to detect a 0.10-point effect at 80% power and α = 0.05, the minimum detectable effect at my actual sample sizes, and simulated surveys of 500 to 50,000 users per period (100 repeats each) to see whether more data shrinks the bias.
- **Tried a correction with AI (Phase 4).** Used the P.R.I.M.E. framework to have Claude write an inverse-probability weighting (IPW) estimator: a logistic model of response on engagement and support tickets, normalized weights, and a bootstrap that refits the model in each replicate. I revised the prompt once so the code used my existing data instead of simulating its own.

## Key Findings
- **The survey overstates the improvement by about seven times.** True mean satisfaction went from 3.290 to 3.386 (+0.096). Respondents reported 3.600 before and 4.304 after (+0.704). The response rate fell from 27.5% to 15.2%, so the after survey heard mostly from satisfied users.
- **The bootstrap is precise around the wrong number.** My hand-written bootstrap matched SciPy: the mean was 3.600 (95% CI 3.585–3.615) before and 4.304 (4.289–4.319) after. Neither interval comes close to the true means. The median was 4 in both periods with a zero-width interval, because with only five possible scores every resample lands on 4.
- **Coverage simulation:** [YOUR RESULT — percentile and BCa coverage with Monte Carlo SE]
- **Significance does not fix bias.** The t-test gave t = 65.17, p < 0.001 and Cohen's d = 0.864. The test answers whether respondents differ, not whether all users improved by 0.70.
- **The survey was never short on data.** Detecting a 0.10-point effect needs about 1,236 respondents per group. I had 13,768 before and 7,597 after, with a minimum detectable effect of 0.036 points and power close to 1.
- **More responses do not reduce the bias.** Across simulated surveys, the bias stayed near 0.58–0.61 points at every size, while the standard deviation of the estimate fell from 0.104 (500 users) to 0.011 (50,000 users). More data made the wrong answer more precise.
- **Weighting only helped a little.** IPW balanced engagement (respondent average engagement_z dropped from 0.339 to 0.002 before, and from 0.729 to 0.011 after), but the corrected improvement was still 0.679 (95% CI 0.655–0.705), far from the true 0.096. Weighting on engagement and tickets cannot correct for response that depends on satisfaction itself, which is missing-not-at-random selection.
- **Recommendation for the PM:** Replace the voluntary post-onboarding survey with a random sample of all users, including those who did not finish onboarding, and follow up with nonrespondents before drawing conclusions about the redesign.

## Tools
Python, NumPy, pandas, SciPy, statsmodels, scikit-learn, Matplotlib · Google Colab
