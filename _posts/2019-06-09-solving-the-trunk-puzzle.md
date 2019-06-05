---
title: Solving the Trunk Puzzle
---

This is my solution to a logic puzzle from [*Discrete Mathematics and Its Applications* by Kenneth H. Rosen](https://www.goodreads.com/book/show/1800803.Discrete_Mathematics_and_Its_Applications), which I recently picked up and am working through. It's exercise 17c from Section 1.2.

## Question

> As a reward for saving his daughter from pirates, the King has given you the opportunity to win a treasure hidden inside one of three trunks. The two trunks that do not hold the treasure are empty. To win, you must select the correct trunk. 

The trunks are inscribed with the messages:

* Trunk 1:  "The treasure is in Trunk 3"
* Trunk 2:  "The treasure is in Trunk 1"
* Trunk 3:  "This trunk is empty"

"Exactly two of the inscriptions are true," says the Queen.

> ... determine whether the Queen who never lies could state this, and if so, which trunk the treasure is in.

## Solution

### Compound Proposition

Let's assign each trunk a propositional variable *p<sub>1</sub>*, *p<sub>2</sub>*, or *p<sub>3</sub>* with a true value if the trunk contains the treasure and a false value if it does not. Using these variable the inscriptions on trunk 1, trunk 2, and trunk 3 can be written *p<sub>3</sub>*, *p<sub>1</sub>*, and ¬*p<sub>3</sub>*, respectively.

The Queen's proposition that "exactly two of the inscriptions are true" can be written in English as "one of either the first inscription is false, or the second inscription is false, or the third inscription is false". That sentence can be expressed as the compound proposition:

(¬*p<sub>3</sub>* ∧ *p<sub>1</sub>* ∧ ¬*p<sub>3</sub>*) ⊕ (*p<sub>3</sub>* ∧ ¬*p<sub>1</sub>* ∧ ¬*p<sub>3</sub>*) ⊕ (*p<sub>3</sub>* ∧ *p<sub>1</sub>* ∧ ¬(¬*p<sub>3</sub>*))

That can be simplified by removing the duplication of ¬*p<sub>3</sub>* and the contradiction (*p<sub>3</sub>* ∧ ¬*p<sub>1</sub>* ∧ ¬*p<sub>3</sub>*). Finally we can remove the double negation ¬(¬*p<sub>3</sub>*) because ¬(¬*p<sub>3</sub>*) ≡ *p<sub>3</sub>*.

(¬*p<sub>3</sub>* ∧ *p<sub>1</sub>* ~~∧ ¬*p<sub>3</sub>*~~) ⊕ ~~(*p<sub>3</sub>* ∧ ¬*p<sub>1</sub>* ∧ ¬*p<sub>3</sub>*) ⊕~~ (*p<sub>3</sub>* ∧ *p<sub>1</sub>* ~~∧ ¬(¬*p<sub>3</sub>*)~~)

The remaining compound proposition is:

(¬*p<sub>3</sub>* ∧ *p<sub>1</sub>*) ⊕ (*p<sub>3</sub>* ∧ *p<sub>1</sub>*)

### Truth Table

Our compound proposition can be used to construct a truth table:

|#|*p<sub>1</sub>*|*p<sub>2</sub>*|*p<sub>3</sub>*|¬*p<sub>3</sub>*|¬*p<sub>3</sub>* ∧ *p<sub>1</sub>*|*p<sub>3</sub>* ∧ *p<sub>1</sub>*|(¬*p<sub>3</sub>* ∧ *p<sub>1</sub>*) ⊕ (*p<sub>3</sub>* ∧ *p<sub>1</sub>*)|
|--|--|--|--|--|--|--|
|1|T|T|T|F|F|T|T|
|2|T|T|F|T|T|F|T|
|3|T|F|T|F|F|T|T|
|4|T|F|F|T|T|F|T|
|5|F|T|T|F|F|F|F|
|6|F|T|F|T|F|F|F|
|7|F|F|T|F|F|F|F|
|8|F|F|F|T|F|F|F|

Examining the truth table, row 4 is the only row that is both satisfiable and where the treasure exists in only one of *p<sub>1</sub>*, *p<sub>2</sub>*, or *p<sub>3</sub>*, or in other words where *p<sub>1</sub>* ⊕ *p<sub>2</sub>* ⊕ *p<sub>3</sub>*. Therefore trunk 1 contains the treasure.