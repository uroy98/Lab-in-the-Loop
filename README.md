# Lab-in-the-Loop

An interactive demonstration of autonomous experimentation for micellar amide couplings,
built on the dataset and model from *ACS Catalysis* 2025, 15, 14207–14214
([10.1021/acscatal.5c03190](https://doi.org/10.1021/acscatal.5c03190)).

A yield-prediction model is connected to a virtual lab holding 28 experimentally measured
reactions. The model predicts, the lab returns the real measurement, and the model
retrains on the difference — with no human decision between experiments.

## Running it

`index.html` is fully self-contained. All data, styling and logic are in that one file.
Open it in any browser, or serve it from anywhere that hosts static pages. No build step,
no server code, no dependencies to install.

## Publishing to GitHub Pages

1. Create a new repository on GitHub.
2. Upload `index.html` and this README to the repository root.
3. Go to **Settings → Pages**.
4. Under **Source**, choose **Deploy from a branch**, select `main` and folder `/ (root)`.
5. Save. After about a minute the site is live at
   `https://<your-username>.github.io/<repository-name>/`

If the page renders oddly, add an empty file named `.nojekyll` to the root — create it
through **Add file → Create new file**, type the name, leave the contents blank, commit.
A single-file site at the root usually does not need it.

## What is in the page

### Run one experiment

Choose a coupling from the virtual lab, set the solvent and temperature, and watch one
full cycle: the model predicts with a confidence range, the lab returns the measured
yield, the gap is shown, and the model retrains on that single result. A panel then
displays what that one experiment did to the model's predictions for all 27 other
reactions — how many improved, how many got worse, and how the average error moved.

Predictions are precomputed across 7 solvents and 4–5 temperatures for every reaction, so
conditions can be changed freely. Conditions that were actually run are marked; anything
else returns a prediction with no measurement to compare against.

### Autonomous experimentation

Five selection rules compete to find the model's weak spots in the fewest bench
reactions. For each round the page shows the actual scoreboard the rule produced, the two
reactions it sent to the lab, and the retraining step, with arrows showing how the
ranking shifted after the model changed. A collapsible worked example scores the same two
reactions under all five rules side by side.

Results are compared over 10 independent repeats per rule, unlocked once all five have
been run. Targeting the reactions the model is least sure about needs about 11 reactions
to find every bad prediction, against about 15 for random selection (paired t-test,
p = 0.006). The three other rules land around 13 and none clearly beats random.

## Honest notes

**This is a retrospective replay.** Every yield was measured at a bench for the published
study; the virtual lab looks them up. Nothing is simulated. Replaying lets the same
reactions be attacked five different ways, which is impossible in a real lab where each
reaction can only be run once.

**11 reactions were excluded.** The measured set contained 39 reactions, of which 11 also
appear in the model's training data — it reproduces those yields to within 0.04 points.
Including them would have flattered the model. That leaves the 28 in the virtual lab.

**Retraining does not improve overall accuracy at this budget.** 28 measurements spread
across 81 distinct acids is too sparse, and partial coverage teaches the model a
confidently wrong average. The demonstrated gain is in choosing which experiments to run,
not in the retraining itself. This is shown in the interface rather than hidden.

**The loop has no stopping rule.** It runs until the candidate pool is exhausted. The
saving in bench time is only realised if a person stops early; automating that decision
is the obvious next step and is not implemented.

**Featurisation differs from the published pipeline.** RDKit was unavailable in the build
environment, so reactions are encoded with component identity, SMILES-derived descriptors
and hashed n-grams rather than Morgan fingerprints. Baseline accuracy on the virtual lab
is R² 0.56, RMSE 18.3.

## Two practical points

**Fonts** load from Google Fonts, so a cold first load needs an internet connection. The
page works offline but falls back to system fonts. Open the live URL once before
presenting so they cache.

**The data is embedded.** All 28 measured yields, the model's predictions and the full
campaign results sit inside `index.html` as JSON, readable by anyone who views the
source. That is fine if the measurements are already published. If any are not, use a
private repository — note that Pages on private repositories requires a paid GitHub plan.
