# Quarto Automation

2024-08-13

- [<span class="toc-section-number">1</span> Key
  Takeaways](#key-takeaways)
- [<span class="toc-section-number">2</span> Talk 1: Quarto
  Automation](#talk-1-quarto-automation)
- [<span class="toc-section-number">3</span> Talk 2: Dynamic Data
  Storytelling](#talk-2-dynamic-data-storytelling)
  - [<span class="toc-section-number">3.1</span> Problem:](#problem)
  - [<span class="toc-section-number">3.2</span> Solution](#solution)
- [<span class="toc-section-number">4</span> Talk 3: Automating Quarto
  with `rdocx`](#talk-3-automating-quarto-with-rdocx)
- [<span class="toc-section-number">5</span> Talk 4: Quarto
  Websites](#talk-4-quarto-websites)
  - [<span class="toc-section-number">5.1</span> Shortcodes: Rendering
    content from other quarto
    documents](#shortcodes-rendering-content-from-other-quarto-documents)

# Key Takeaways

- Can create conditional emails only when needed in Quarto/Posit Connect
  with Pins

- Can add interactivity to websites with \`css

- Can include other parts of quarto docs by using `embed` or `include`

# Talk 1: Quarto Automation

- Quarto can render preexisting `html` code

- `rsync` can help render and upload a website.

- Can add `css` code that will allow for theming like interactivity
  (e.g., highlight on hover)

# Talk 2: Dynamic Data Storytelling

## Problem:

- Stakeholder often requests this information, but stakeholder does not
  have enought time to process the information

- Too much data leading to analysis parallysis

## Solution

> “no click insights” woth Quarto for custom emails tailored to a person

- Personalize the delivery: only send the email when needed, such as
  conditional logic in the code and only sends to the individual when
  needed.

- You can create data output and send them to pins

<!-- -->

    format: email

    <!-- code here -->

    <!-- conditional email logic -->

    <!-- email body -->

The distribution group can be added via user groups.

# Talk 3: Automating Quarto with `rdocx`

> Mass produce R markdown output in word documents, which eliminates
> copying and pasting into a word document

# Talk 4: Quarto Websites

## Shortcodes: Rendering content from other quarto documents

> Include other parts of qmd’s into your analysis

> Eliminates copying and pasting

    {{< include file.qmd >}} # includes everything and renders a quarto doc. Can use # for title blocks
    {{< embed file.qmd >}}   # includes similar aspects?
