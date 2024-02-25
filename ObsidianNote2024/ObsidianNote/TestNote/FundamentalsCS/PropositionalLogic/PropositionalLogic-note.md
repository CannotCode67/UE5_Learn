What is propositional logic(命题逻辑)? 

It is a branch of logic for studying mathematical statement. 

It is the base of predicate logic which is even more powerful for modeling reasoning. 

It is effectively an algebra of propositions. In this algebra, the variables are unknown propositions instead of unknown real numbers. 

The operators used are: and, or, not, implies, if and only if, rather than +, -, *, /. 

What is a proposition? 

It is a declarative sentence that is either true or false, but not both. 

It is the most basic element of logic, the building block of reasoning. 

To avoid writing long and repetitive propositions, we use propositional variables to denote the propositions, which is just a letter such as: p, q, r,etc. 

p: London is the capital of the United Kingdom 

p is true. 

q: 1 + 1 > 3 

q is false. 

Simple propositions like the ones above are easy to figure out true or false. However, when we have complex propositions, we need a methodology to help keeping track and it's the truth table.  

To construct the truth table for n propositions: create a table with 2^n rows and n columns.  

Below it is a example. We have 3 propositions: P, Q, R, so 2^3 = 8 rows.

![[FundamentalsCS/PropositionalLogic/PropositionalLogic-images/GetImage.png]]

logic operators (connectives):

![[FundamentalsCS/PropositionalLogic/PropositionalLogic-images/GetImage (1).png]]![[FundamentalsCS/PropositionalLogic/PropositionalLogic-images/GetImage (2).png]]![[FundamentalsCS/PropositionalLogic/PropositionalLogic-images/GetImage (3).png]]![[FundamentalsCS/PropositionalLogic/PropositionalLogic-images/GetImage (5).png]]![[FundamentalsCS/PropositionalLogic/PropositionalLogic-images/GetImage (4).png]]![[FundamentalsCS/PropositionalLogic/PropositionalLogic-images/GetImage (6).png]]
![[FundamentalsCS/PropositionalLogic/PropositionalLogic-images/GetImage (7).png]]
![[FundamentalsCS/PropositionalLogic/PropositionalLogic-images/GetImage (8).png]]
![[FundamentalsCS/PropositionalLogic/PropositionalLogic-images/GetImage (9).png]]
![[FundamentalsCS/PropositionalLogic/PropositionalLogic-images/GetImage (10).png]]

When  two propositions are logically equivalent, it means they always have the same truth value, which can be proved by using truth table. 

Operators have following precedence.
![[FundamentalsCS/PropositionalLogic/PropositionalLogic-images/GetImage (11).png]]

Some propostions are always true, they are called tautology. 

Some propositions are true at least for one scenario, they are called consistent. 

Some propositions are always false, they are called inconsistent or contradiction.

![[FundamentalsCS/PropositionalLogic/PropositionalLogic-images/GetImage (12).png]]
![[FundamentalsCS/PropositionalLogic/PropositionalLogic-images/GetImage (13).png]]
![[FundamentalsCS/PropositionalLogic/PropositionalLogic-images/GetImage (14).png]]
![[FundamentalsCS/PropositionalLogic/PropositionalLogic-images/GetImage (15).png]]
![[FundamentalsCS/PropositionalLogic/PropositionalLogic-images/GetImage (16).png]]
negate and = or 

negate or = and