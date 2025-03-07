---
layout: page
title: Workspace ONE UEM API
hide:
  #- navigation
  - toc
---
![Workspace ONE UEM](../../../assets/logos/UEM-v-lm.png){ align=right }

Each Workspace ONE UEM tenant provides an API/help page that displays all the API commands, parameters and usage, as well as a mechanism to test each call. The use of and getting started is described in the [Let’s Git Commit(ted) to </Dev> Resources: Getting Started with the Workspace ONE UEM REST APIs](https://techzone.omnissa.com/blog/lets-git-committed-resources-getting-started-workspace-one-uem-rest-apis) article.

This process of testing APIs on the API/help page was cumbersome, especially when having to then manually build the same API call in Postman. It was time-consuming and prone to errors.

Omnissa has recently introduced a comprehensive Postman collection for Workspace ONE UEM. This collection includes all available API calls, pre-configured and ready to use, significantly improving workflow efficiency.

## Getting Started

Utilising a Postman collection is easy and an exceptionally fast way to get started. If you are new to working with APIs or Postman, see the [Workspace ONE UEM APIs Overview](https://www.postman.com/workspace-one-uem/workspace-one-uem-apis/overview) in the [Postman](https://www.postman.com/) collection to help you get started.
![](Workspace%20ONE%20UEM%20APIs%20Overview.png)

The high-level process documented in the **Workspace ONE UEM APIs** overview link above are:

1. Create Fork from Existing Collection
2. Collect your tenant values
3. Create an *environment* and add your tenant values as variables, including authentication
4. Use the APIs that match your tenant version
