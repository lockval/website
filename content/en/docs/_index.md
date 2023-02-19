
---
title: "Documentation"
linkTitle: "Documentation"
weight: 20
menu:
  main:
    weight: 20
---

Basic idea

The Lockval engine treats the entire environment (including front-end data) as one ACID database. When you write code that changes the value. The modified data will be synchronized to the background database and also to the front end.

Lockval Engine base architecture:

![architecture](/arch.svg "base architecture")

