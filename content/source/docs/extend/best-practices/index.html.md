---
layout: "extend"
page_title: "Extending Terraform: Best Practices"
sidebar_current: "docs-extend-best-practices"
description: |-
  Extending Terraform is a section for content dedicated to developing Plugins
  to extend Terraform's core offering.
---

# Terraform Plugin Best Practices

A key feature of Terraform is it’s plugin system, separating the logic of
managing state and configuration and providing  safe plan and apply lifecycle
management, from the details of specific vendor APIs. Plugins are responsible
for the  implementation of functionality for provisioning resources for a
specific cloud provider, allowing each Provider to fully support it’s unique
resources and lifecycles and not settling for the lowest common denominator
across all Provider resources of that type (virtual machines, networks,
configuration management systems, et. al). While each Provider is unique, over
the years we’ve accumulated some patterns that should be adhered to in order to
ensure a consistent user experience when using Terraform for any given provider.
Listed below are a few Best Practices we’ve found to generally apply most
Providers, with a brief description of each, and link to read more. Each
practice is also linked in navigation on the left.

The Best Practices section is a work in progress, with more sections to come.

## Detecting Drift

Terraform is a declarative tool designed to be the source of truth for
infrastructure. In order to safely and predictably change and iterate
infrastructure, Terraform needs to be able to detect changes made outside of
it's configuration and provide means of reconciliation. In [Detecting
Drift][drift], we cover some best practices to ensure Terraform's statefile is
an accurate reflection of reality, in order to provide accurate plan and apply
functionality. 

[Click here to learn more.][drift]

[drift]: /docs/extend/best-practices/detecting-drift.html
