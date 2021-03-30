+++
widget = "featured"
headless = true  # This file represents a page section.

# ... Put Your Section Options Here (title etc.) ...


active = false  # Activate this widget? true/false
weight = 10  # Order that this section will appear.


[content]
  # Page type to display. E.g. post, event, or publication.
  page_type = "post"
  # Choose how much pages you would like to display (0 = all pages)
  count = 0
  # Page order. Descending (desc) or ascending (asc) date.
  order = "desc"
  # Optionally filter posts by a taxonomy term.
  [content.filters]
    tag = ''
    category = ''
    publication_type = ''
[design]
  # Toggle between the various page layout types.
  #   1 = List
  #   2 = Compact
  #   3 = Card
  #   4 = Citation (publication only)
  view = 3
+++