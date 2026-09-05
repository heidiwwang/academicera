---
type: {{itemType}}
citekey: {{citekey}}
title: "{{title}}"
concepts:
{%- set seen = "||" -%}
{%- for annotation in annotations -%}
  {%- for t in annotation.tags -%}
    {%- set clean_tag = t.tag | lower | replace("#", "") | replace("(", "") | replace(")", "") | replace("&", " ") | replace("/", " / ") | replace("-", " ") | replace("  ", " ") | trim | replace(" ", "_") | replace("__", "_") | replace("_/_", "/") | replace("/_", "/") | replace("_/", "/") -%}
    {%- set tag_token = "|" + clean_tag + "|" -%}
    {%- if tag_token not in seen and clean_tag %}
  - "[[{{ clean_tag }}]]"
      {%- set seen = seen + clean_tag + "||" -%}
    {%- endif -%}
  {%- endfor -%}
{%- endfor %}
year: "{% if date %}{{ date | format("YYYY-MM-DD") }}{% else %}Unknown{% endif %}"
authors:
{%- set raw_authors = authors if authors else (bookAuthors if bookAuthors else (publisher if publisher else (institution if institution else (company if company else "Unknown")))) -%}
{%- set normalized_authors = raw_authors | replace(" and ", ", ") -%}
{%- for author in normalized_authors.split(",") %}
  - "{{ author | trim }}"
{%- endfor %}
tags:
{%- for t in tags %}
  {%- set clean_item_tag = t.tag | lower | replace("#", "") | replace("(", "") | replace(")", "") | replace("&", " ") | replace("/", " / ") | replace("-", " ") | replace("  ", " ") | trim | replace(" ", "_") | replace("__", "_") | replace("_/_", "/") | replace("/_", "/") | replace("_/", "/") %}
  - {{ clean_item_tag }}
{%- endfor %}
---
# {{title}}
> [!info] 
> {{bibliography}}
{% if abstractNote %}
## Summary
{{abstractNote}}
{% endif %}
## Annotations
{% for annotation in annotations | selectattr("tags") | sort(attribute="tags.tag") %}
{%- if annotation.annotatedText and annotation.tags.length > 0 %}
> [!note] Page {{ annotation.pageLabel if annotation.pageLabel else "Note" }} — {% for t in annotation.tags %}[[{{ t.tag | lower | replace("#", "") | replace("(", "") | replace(")", "") | replace("&", " ") | replace("/", " / ") | replace("-", " ") | replace("  ", " ") | trim | replace(" ", "_") | replace("__", "_") | replace("_/_", "/") | replace("/_", "/") | replace("_/", "/") }}{% if not loop.last %}]], {% endif %}{% endfor %}]]
> '{{ annotation.annotatedText | replace("\n", "'\n> '") }}'

{%- endif %}
{% endfor %}