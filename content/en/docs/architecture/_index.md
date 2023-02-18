
---
title: "Architecture"
linkTitle: "Architecture"
weight: 8

---

Lockval Engine integrates gw (provides the function of synchronizing data with the front end), db (implements acid database) and trigger (provides a method for external access to Locval Engine). Go plugins provide additional extensions. In this way, you only need to focus on the development of code logic.

Lockval Engine base architecture:

![architecture](/arch.svg "base architecture")

