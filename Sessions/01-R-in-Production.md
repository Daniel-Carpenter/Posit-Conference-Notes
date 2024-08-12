# Posit Conference Notes

2024-08-12

- [<span class="toc-section-number">1</span> Session: R in
  Production](#session-r-in-production)
  - [<span class="toc-section-number">1.1</span> Key
    Takeaways](#key-takeaways)
  - [<span class="toc-section-number">1.2</span> Misc.
    Facts](#misc-facts)
- [<span class="toc-section-number">2</span> The Whole
  Game](#the-whole-game)
  - [<span class="toc-section-number">2.1</span> GitHub
    Actions](#github-actions)
  - [<span class="toc-section-number">2.2</span> Posit
    Connect](#posit-connect)
  - [<span class="toc-section-number">2.3</span> Logging](#logging)
  - [<span class="toc-section-number">2.4</span>
    Authentication](#authentication)
  - [<span class="toc-section-number">2.5</span> Running Code
    Repeatedly](#running-code-repeatedly)

# Session: R in Production

## Key Takeaways

- GitHub Actions to automate batch jobs (only for )

- [`pointblank`](https://rstudio.github.io/pointblank/) package for data
  validation

  - (validation report, column types, missingess, values in range)

## Misc. Facts

- bacth: anything that is going to run at an interval, such as a
  database update.

- Deploy code, vs. render

  - `pak::pak`: faster than `install.packages()` since runs in parallel,
    does dependancies, generally easy and fast.

    - Use `rig` to install R

    - Use `PPPM` for sourcing public binaries.

- In a project, use the `R/` folder for the reused code

- Remember that the server runs on UTC

- Use the `ragg` package on linux to produce the same level of graphics.

# The Whole Game

- “You cannot do data science if you do not understand Git”

## GitHub Actions

- Description: Able to run jobs, like R, python scripts, shiny apps,
  integrates well with cron (cron generated with LLM, verified with cron
  guru, add comment)

### Benefits

- Auto-Commits back to GitHub after running the code

- Great for batch jobs

### Cons

- Must be run every 60 days or else it will stop running.

### Tips:

- Find a template or create one on ChatGPT, no one really writes these
  by hand.

- Use the same naming before the file extension

- Pick a random minute in the hour to schedule the job because most do 0
  or 5’s

- You will need to install every package that the code will run, within
  the GitHub action

- Waiting a bit can help

## Posit Connect

- Key use case: shiny apps

- better dependancy installation system

## Logging

- In production code, a good idea would be to add log messages
  throughout the code, such as using `cat('Message', file = sterr())`.
  Also, could do informative chunk naming.

- The `logger` package enhaces this by adding warnings, errors, etc,
  while integrating variables through `glue`

## Authentication

- Federated authetication trumps enctryted authentication. However, your
  IT team sets up federated. Posit Connect allows for this.

### Secrets

- GitHub, can add secrets per repo
- Can add secrets on Posit Connect
- Note, do not commit .Rhistory files because that is a log of the
  console, which could send secret info.

## Running Code Repeatedly

- Eliminate all warnings and messages

  - Warnings remove. could use `options(warn = 2)` to change all
    warnings for errors.

  - Deprication could change this to an error with
    `options(lifecycle_verbosity = "error")`

  - Messages help clean up the logs for error isolation

- Check and reduce the number of dependencies, causing potential for
  errors in the future

  - `pak` packages has a packages needed that we can evakuate

### Dealing with Package Conflicts:

Use the `library(conflicts)` function, and it will throw and error and
force you to deal with the conflicts

### `renv`: Install a package per project

> Only use this unless you have to, keeps the code in the original
> state.

> Problem: This could cause stale packages

### Data Changes

Social: make friends and contracts with your data.

Technical: make error checks

#### `pointblank`:

check’s column types, missingness, matching variables, values are in
range

RMD data validation report - Data quality reporting  
- Pipeline data validation  
- Expectations in unit tests  
- Custom control flow  
- Rmd integration

Can send emails on fail using connect

<https://github.com/posit-conf-2023/ds-workflows-r/blob/main/materials/project/01_data_clean_validate/01_data_clean_validate.rmd#L192C3-L192C21>
