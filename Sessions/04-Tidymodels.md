What’s new with tidymodels
================
8/13/24

- <a href="#time-to-event" id="toc-time-to-event"><span
  class="toc-section-number">1</span> Time to event</a>
  - <a href="#explanation" id="toc-explanation"><span
    class="toc-section-number">1.1</span> Explanation</a>
  - <a href="#links-to-sessions" id="toc-links-to-sessions"><span
    class="toc-section-number">1.2</span> Links to sessions</a>
- <a href="#fair-machine-learning" id="toc-fair-machine-learning"><span
  class="toc-section-number">2</span> Fair machine learning</a>
- <a href="#tidypredict-and-recipes"
  id="toc-tidypredict-and-recipes"><span
  class="toc-section-number">3</span> Tidypredict and Recipes</a>
  - <a href="#orbital" id="toc-orbital"><span
    class="toc-section-number">3.1</span> Orbital</a>

# Time to event

## Explanation

> Predict the probability of an event happening at different times,
> considering various factors that might influence the timing.

Time-to-event models, also known as survival models, are statistical
methods used to analyze the time until a specific event occurs, such as
failure, death, or recovery. They help predict the probability of an
event happening at different times, considering various factors that
might influence the timing.

Key points: - **Focus on Time:** These models analyze the duration until
an event happens. - **Events:** Commonly used to study events like
equipment failure, patient survival, or customer churn. - **Censoring:**
Accounts for incomplete data, like when the event hasn’t happened by the
study’s end. - **Hazard Function:** Describes the risk of the event
occurring at a specific time.

Time-to-event models are widely used in fields like medicine,
engineering, and finance for predicting outcomes over time.

## Links to sessions

- [Survival analysis with
  tidymodels](https://hfrick.github.io/2024-posit-conf/)

- [Evaluating time-to event
  models](https://topepo.github.io/2024-posit-conf/#/classificationish-metrics)

# Fair machine learning

<https://simonpcouch.github.io/conf-24/#/>

- A badly tuned model could lead to less equitable outcomes for people

- A badly tuned model could mean under valued or overvalued leading to
  incorrect results

# Tidypredict and Recipes

- Can run models and create predictions then insert them to a database

## Orbital

- Take the tidymodel and concisely display what happened (communicate)

- Note very new, and not all models work this way. Tree and linear
  models work.
