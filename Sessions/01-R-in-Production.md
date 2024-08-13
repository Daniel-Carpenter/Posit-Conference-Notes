# R in Production

2024-08-12

### Key Takeaways

#### 1. **Production Code**

- **Automation with GitHub Actions:**
  - Automate batch jobs using GitHub Actions for repetitive tasks.

  - Ensure the setup includes dependency management, correct timing with
    cron jobs, and periodic maintenance (every 60 days).

  - **Example:**

    ``` yaml
    jobs:
      scrape:
        runs-on: ubuntu-latest
        permissions:
          contents: write
        steps:
          - uses: actions/checkout@v4
          - uses: r-lib/actions/setup-r@v2
            with:
              use-public-rspm: true
          - uses: r-lib/actions/setup-r-dependencies@v2
          - name: Fetch latest data
            run: Rscript scrape.R
          - name: Collapse into yearly parquet files
            run: Rscript collapse.R
          - uses: stefanzweifel/git-auto-commit-action@v5
    ```

  - **Resource:** [GitHub Actions
    Examples](https://github.com/r-lib/actions/tree/v2-branch/examples)
- **Data Validation with `pointblank`:**
  - Validate data types, handle missing values, and ensure data
    integrity.

  - Integrate with RMarkdown for detailed validation reports and
    automated checks.

  - **Example:**

    ``` r
    library(pointblank)
    data |> 
      col_is_date(date) |>
      col_is_numeric(temperature) |>
      col_vals_not_null(date) |>
      col_vals_between(temperature, 30, 120)
    ```

  - **Resource:** [`pointblank`
    Package](https://rstudio.github.io/pointblank/)
- **Logging:**
  - Use logging (`logger` package) to track and diagnose issues in
    production code.

  - Log key events, warnings, and errors to streamline debugging.

  - **Example:**

    ``` r
    library(logger)
    log_info("🛫 Script starting up.....")
    log_info("Processing {nrow(df)} rows")
    log_warn("❌ Missing data for {length(problems}) variables")
    log_info("🛬 Completed; wrote {length(files)} files")
    ```

  - **Resource:** [Logger Package
    Example](https://github.com/hadley/houston-pollen/actions/runs/10101725281/job/27935890005)

#### 2. **Code Management and Collaboration**

- **Shared Coding Practices:**
  - Establish a team coding style guide (e.g., Tidyverse style guide).
  - Promote shared code ownership through GitHub, with regular pull
    requests and code reviews.
  - **Examples:**
    - [Tidyverse Style Guide](https://style.tidyverse.org/)
    - [R Packages by J. Leek](https://github.com/jtleek/rpackages)
- **Package and Dependency Management:**
  - Utilize `pak` for efficient package management and `renv` for
    project-specific libraries.

  - Monitor and eliminate unnecessary dependencies to reduce code
    fragility.

  - **Example:**

    ``` r
    renv::snapshot()
    ```

  - **Resource:** [Dependency Management
    Insights](https://www.tidyverse.org/blog/2019/05/itdepends/)

#### 3. **Running Code on Servers**

- **Server-Specific Considerations:**
  - Understand differences in environments (local vs. server) and manage
    issues like timezone, locale, and system dependencies.
  - Use containers to standardize environments and ensure consistency
    across deployments.
  - **Resource:** [Rocker Containers for
    R](https://github.com/rocker-org/rocker)
- **Authentication and Security:**
  - Implement secure authentication methods, favoring federated
    authentication where possible.

  - Manage environment variables carefully to avoid exposing sensitive
    information.

  - **Example:**

    ``` r
    Sys.getenv("NEWS_API_KEY")
    ```

  - **Resource:** [How to Manage Secrets in
    GitHub](https://newsapi.org/)

#### 4. **Continuous Improvement**

- **Refactoring and Maintenance:**
  - Regularly refactor code to improve readability and maintainability.
  - Address warnings and errors early to maintain clean, error-free
    logs.
  - **Resource:** [Code Review
    Principles](https://code-review.tidyverse.org/)
- **Adaptation to Change:**
  - Anticipate and adapt to changes in data, schema, or platform by
    implementing robust validation and error-handling mechanisms.
  - **Resource:** [Data Validation Advanced
    Workflows](https://github.com/posit-conf-2023/ds-workflows-r/blob/main/materials/project/01_data_clean_validate/01_data_clean_validate.rmd#L192C3-L192C21)

------------------------------------------------------------------------

### Actionable Steps

1.  **Set Up GitHub Actions for Automation:**
    - Implement workflows to automate repetitive tasks, ensuring they
      include necessary package installations and proper scheduling.
    - **Resource:** [GitHub Actions
      Setup](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions)
2.  **Enhance Data Integrity:**
    - Integrate `pointblank` into your data pipelines for continuous
      data validation and error reporting.
    - **Example:** [pointblank RMD
      Integration](https://github.com/posit-conf-2023/ds-workflows-r/blob/main/materials/project/01_data_clean_validate/01_data_clean_validate.rmd#L192C3-L192C21)
3.  **Standardize and Share Code:**
    - Create a team coding style guide and enforce it through regular
      code reviews and shared GitHub repositories.
    - **Resources:**
      - [Tidyverse Style Guide](https://style.tidyverse.org/)
      - [Code Review Guide](https://code-review.tidyverse.org/)
4.  **Optimize Server Deployments:**
    - Use containers to match development and production environments,
      and manage system dependencies effectively.
    - **Resource:** [Rocker
      Containers](https://github.com/rocker-org/rocker)
5.  **Improve Logging Practices:**
    - Implement structured logging across all production code to aid in
      troubleshooting and monitoring.
    - **Example:** [Logger Package
      Usage](https://github.com/hadley/houston-pollen/actions/runs/10101725281/job/27935890005)
6.  **Refactor Regularly:**
    - Schedule regular code reviews and refactoring sessions to ensure
      your codebase remains clean and efficient.
    - **Resource:** [Refactoring
      Techniques](https://developer-success-lab.gitbook.io/code-review-anxiety-workbook-1)

------------------------------------------------------------------------

This version incorporates your original code snippets and links,
ensuring the content is both actionable and detailed for future
reference.

<br>

------------------------------------------------------------------------

Here is a concise summary of the key points from the slides:

### **1. Whole Game Overview**【10†source】:

- **Definition of “In Production”**:
  - Code runs on another machine (typically a Linux server).
  - Code runs repeatedly (scheduled or on-demand).
  - Code and data are shared responsibilities.
- **Categories of Production Jobs**:
  - **Batch**: R scripts, reports, models (scheduled, computationally
    intensive).
  - **Interactive**: Shiny apps, APIs (on-demand, lighter computation).
- **Deployment Tools**:
  - **GitHub Actions**: Suitable for batch jobs, integrates with Git.
  - **Posit Connect Cloud**: Best for interactive jobs, handles
    deployment details.
  - **DIY**: Use Docker, rig, pak for custom setups.

### **2. Running Code on Another Server**【9†source】:

- **Challenges**:
  - **Minor Frustrations**: Differences between local (Windows/Mac) and
    server (Linux) environments (e.g., time zones, fonts).
  - **Package Management**: Ensuring consistent package versions across
    environments.
  - **Debugging & Logging**: Using tools like `rlang::back_trace()` for
    effective error handling.
  - **Authentication**: Managing environment variables securely across
    platforms.
- **Tools and Tips**:
  - **Containers**: Insulate platform changes using Docker.
  - **Logging**: Include detailed logs to simplify debugging.
  - **System Dependencies**: Use `pak::pkg_sysreqs()` to identify
    missing libraries.

### **3. Running Code Repeatedly**【8†source】:

- **Change Management**:
  - **Data and Schema Changes**: Regular updates can break code;
    establish robust checks.
  - **Package and Platform Updates**: Capture versions at deployment to
    maintain stability.
  - **Validation**: Use tools like `pointblank` to validate data
    integrity.
- **Mitigation Strategies**:
  - **Containers**: Isolate dependencies to prevent issues.
  - **Renv**: Create project-specific libraries to ensure
    reproducibility.
  - **Refactoring**: Regularly update and clean up code to handle
    evolving requirements.

### **4. Shared Responsibility**【7†source】:

- **Sharing Code and Data**:
  - **Data Formats**: Use efficient formats like Parquet for sharing.
  - **Git Repositories**: Establish naming conventions and version
    control practices.
  - **Code Reviews**: Implement regular code reviews for quality
    control.
  - **Team Packages**: Develop shared libraries for common tasks.
- **Tools and Best Practices**:
  - **GitHub**: Centralize code and documentation for collaboration.
  - **Documentation**: Build and maintain a team style guide.
  - **Automated Workflows**: Use tools like `targets` and `pins` to
    automate tasks.

Each section focuses on essential concepts and best practices for
managing R code in production environments, emphasizing consistency,
automation, and collaboration across teams.
