---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - csharp
  - java

toc_footers:
  - <a href='https://365view.io' target='_blank'>Discover 365view</a>
  - <a href='https://github.com/365view/slate' target='_blank'>Contribute to this documentation</a>
  - <a href='https://github.com/lord/slate' target='_blank'>Documentation Powered by Slate</a>

includes:
# Common pages go here
  - core/auth
  - core/menu
  - core/generic
  - core/errors
# Add the connector pages here. Ordering matters
# 365 repo
  - 365/intro
  - 365/auth
# Ora 12
  - ora12/intro
  - ora12/auth

search: true
---

# Introduction

Welcome to the 365View API beta! You can use our API to access 36View API endpoints, which can be used to interact with the 365View repository or your administered platforms through the 365View connectors.

This API documentation page was created with [Slate](https://github.com/lord/slate).

<aside class="success">Most exchange with connectors are made through JSON objects. The type of object is defined by the field *kind* and each method only accepts certain kinds. Each connector must indicate for each method the accepted kinds.</aside>