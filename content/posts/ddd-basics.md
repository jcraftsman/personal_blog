---
title: "Ddd Basics"
date: 2020-03-19T10:01:48+01:00
draft: true
---

## Useful links

- <https://vladikk.com/2018/01/26/revisiting-the-basics-of-ddd/>
- <https://blog.sapiensworks.com/post/2015/11/23/DDD-is-not-programming>
- <https://www.youtube.com/playlist?list=PLIHVWAtJFACm4sffK5M_UxDg7k-xXI16F>

- <https://philippe.bourgau.net/3-good-and-bad-ways-to-write-team-coding-standards-and-conventions/>

- <https://github.com/heynickc/awesome-ddd> 🌟🌟🌟

- <https://medium.com/@nikeshshetty/designing-the-ddd-way-introduction-9acd910e418> :
Invariants — Invariants are generally business rules/enforcements/requirements that you impose to maintain the integrity of an object at any given time.
The crux of DDD lies in the approach you take towards the problem statement. And most often, it’s not always about only having a domain knowledge but also understanding/questioning the business needs holds the key to DDD style of functioning.
DDD talks very closely with the business and businesses change requirements to survive every quarterly results tide. Every change in invariants results in your evolving domains, resulting in the need to refactor the code to respect the invariants.
And the best way to understand it by citing an example.
A bank has many customers and each customer can open a bank account. The account should be removed and not accessible the moment it’s account balance becomes 0. (looks like this bank serves underground agents :P )Customer is identified with his social security number(SSN). The other information is his address which should have line1,city,country.
Enter the DDD world, and you will say


**On ne peut pas créer d'objets dans un état invalide**

> It's not stakeholder knowledge but developers' ignorance that gets dployed into production
> Alberto Brandolini


Ubiquitous languange


One model to rule them all !!! (Model donmain specific vs universal)

