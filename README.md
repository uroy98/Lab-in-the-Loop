# Lab-in-the-Loop

An interactive demonstration of autonomous experimentation for micellar amide couplings,
built on the dataset and model from *ACS Catalysis* 2025, 15, 14207–14214.

A yield-prediction model is wired to a virtual lab holding 28 experimentally measured
reactions. The model predicts, the lab returns the real measurement, and the model
retrains on the difference — with no human decision between experiments.

## Running it

`index.html` is fully self-contained: all data, styling and logic are in that one file.
Open it in any browser, or serve it from anywhere that can host a static page. There is
no build step and no server code.

## Publishing to GitHub Pages

1. Create a new repository on GitHub.
2. Upload `index.html`, `.nojekyll` and this file to the repository root.
3. Go to **Settings → Pages**.
4. Under **Source**, choose **Deploy from a branch**, select `main` and folder `/ (root)`.
5. Save. After about a minute the site is live at
   `https://<your-username>.github.io/<repository-name>/`

`.nojekyll` stops GitHub from running its static-site processor over the file, which is
unnecessary here and can interfere with plain HTML.

## Two things worth knowing

**Fonts.** Typefaces load from Google Fonts, so a cold first load needs an internet
connection. The page works offline but falls back to system fonts. Open the live URL
once before presenting so the fonts cache.

**The data is embedded.** All 28 measured yields, the model's predictions and the full
campaign results are inside `index.html` as JSON, readable by anyone who views the
source. This is fine if the underlying measurements are already published, but a public
repository does make them trivially downloadable. Use a private repository if that
matters — note that Pages on private repositories requires a paid GitHub plan.

## What is in the page

- **Run one experiment** — pick a coupling and conditions, watch the model predict, the
  lab answer, and the model retrain, with the effect on all 27 other predictions shown.
- **Autonomous experimentation** — five selection rules compete to find the model's
  failure modes in the fewest bench reactions, with the per-round scoring, the retraining
  step, and a 10-repeat comparison of the rules.
