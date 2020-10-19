---
title: "Kata Gilded Roses"
date: 2020-05-05T18:42:56+02:00
draft: true
---

https://github.com/emilybache/GildedRose-Refactoring-Kata

Un kata :
Un geste maitrisé. But aquérir des réflexes.


Je vais répéter une des résolution les plus populaires.

Ce que je vais montrer :

- Faire des changements de manière oncémentale en restant le plus proche de la barre verte
- Introduire de la compléxité pour l'enlever par la suite
- Introduire de la duplication pour l'enlever par la suite
- Make the change easy then make the easy change
- Avancer à pas de chaton : faire de changements maitrisés. Laisser l'IDE coder. Il a moins de chance de se tromper.
- Vu que je vais introduire des techniques contre-intuitive, je vous invite à prendre note de vos remarques et questions et on en discutera à la fin du live coding.

1. test
1. extract method: updateOneItem
1. extract method: updateQuality
1. extract method: updateSellIn
1. extract method: updateExpired
1. extract method: hasExpired
1. invert if (tous les if not)
1. extract method: updateCheeseOrConcert
1. Faire emerger un pattern : swap l'ordre des ifs (if name...)
1. Chercher des concepts cachés: chercher des similarités
1. extract method: incrementQuality
1. Dupliquer (if name =="aged Brie || backstage) pour simplifier après
1. inline updateCheeseOrConcert puis ne garder que ce qui est utiliser dans chaque partie. (isolate and inline improve pattern)
1. updateQuality: if name .... => merge ifs
1. updateQuality: if name .... => remove return
1. faire la même chose avec updateExpired
1. faire la même chose avec updateExpired + simplify quality
1. extract method decrementQuality
1. Je detecte une symetrie (incrementquality decrementQuality) => Je réduis la distance visuelle entre les 2 méthodes
1. Remaruqe qu'il existe un certain nombre de méthodes qui prennent en paramètre un `Item` : Je pourrais move F6 incrementQuality to Item class
1. Mais... Je remarque qu'il y a quand même que mes items ont des règles de calculs différentes. je vais plutot introduire cette notion de catégorie (encore une fois, là j'invente des mots. Dans la vie réelle c'est l'opportunité de découvrir des notions métiers). => `ItemCategory category= categorize(item)`
1. cmd+F6 : ajouter ItemCategory à la signature de chaque méthode
1. Maintenant = F6 à toutes les méthodes vers ItemCategory
1. categorize returns Legendary if name=="Sulfuras..."
1. make methods in ItemCategory protected
1. duplicate implementation
1. Simplify Legendary
1. Simplify ItemCategory
1. Faire pareil avec Aged Brie
1. Faire pareil avec les autres
1. Implémenter la nouvelle feature "CONJURED"
1. Changer test => rouge
1. Ajouter categorie CONJURED
1. Finir l'implémentation

https://www.youtube.com/watch?v=HMvue3TMgSk&t=175s
