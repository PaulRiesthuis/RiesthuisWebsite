---
date: "2022-10-24"
sections:
- block: about.biography
  content:
    title: Biography
    username: admin
  id: about
- block: collection
  content:
    filters:
      featured_only: true
      folders:
      - publication
    text: |-
      {{% callout note %}}
      See all [publications](./publication/) here.
      {{% /callout %}}
    title: Featured Publication
  design:
    columns: "2"
    view: card
  id: featured
- block: collection
  content:
    count: 5
    filters:
      author: ""
      category: ""
      exclude_featured: false
      exclude_future: false
      exclude_past: false
      folders:
      - post
      publication_type: ""
      tag: ""
    offset: 0
    order: desc
    subtitle: In this section, I will provide some data analyses, tutorials, and fun R stuff.
    text: |-
      {{% callout note %}}
      See all [posts](./post/) here.
      {{% /callout %}}
    title: Posts
  design:
    columns: "2"
    view: compact
  id: posts
  design:
    columns: "1"
    flip_alt_rows: false
    view: showcase
  id: projects
- block: collection
  content:
    filters:
      folders:
      - event
    text: |-
      {{% callout note %}}
      See all [Talks](./event/) here.
      {{% /callout %}}
    title: Talks
  design:
    columns: "2"
    view: compact
  id: talks
- block: experience
  content:
    title: CV
    subtitle: Experience
    items:
      - title: CEO
        company: GenCoin
        company_url: ''
        company_logo: org-gc
        location: California
        date_start: '2021-01-01'
        date_end: ''
        description: |2-
            Responsibilities include:
        
            * Analysing
            * Modelling
            * Deploying
      - title: Professor of Semiconductor Physics
        company: University X
        company_url: ''
        company_logo: org-x
        location: California
        date_start: '2016-01-01'
        date_end: '2020-12-31'
        description: Taught electronic engineering and researched semiconductor physics.
    design:
      # Choose how many columns the section has. Valid values: '1' or '2'.
      columns: '2'
    subtitle: Experience
  id: experience
title: null
type: landing
---
