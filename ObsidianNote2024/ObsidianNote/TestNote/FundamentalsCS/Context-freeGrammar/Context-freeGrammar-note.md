Regular expression is used to describe sequences of characters, but some sequences are not regular, thus cannot be described by regular expression or accepted by finite automata. Context-free grammar (CFG) can be used to express those sequences. The context-free grammar is one of many kinds of grammar. 

The relationship between regular languages and context-free languages.
![[FundamentalsCS/Context-freeGrammar/Context-freeGrammar-images/GetImage.png]]

Define context-free grammar conceptually
![[FundamentalsCS/Context-freeGrammar/Context-freeGrammar-images/GetImage (1).png]]

What does context-free grammar look like
![[FundamentalsCS/Context-freeGrammar/Context-freeGrammar-images/GetImage (2).png]]
With an example
![[FundamentalsCS/Context-freeGrammar/Context-freeGrammar-images/GetImage (3).png]]
Grammar(G): S -> bSa (rule 1), S -> ba (rule 2) 

Sequences described by this grammar would be ba, bbaa, bbbaaa…… 


How to generate sequences from this context-free grammar?(call derivation)
![[FundamentalsCS/Context-freeGrammar/Context-freeGrammar-images/GetImage (4).png]]
With an example
![[FundamentalsCS/Context-freeGrammar/Context-freeGrammar-images/GetImage (5).png]]


All the sequences derived from a grammar is called the language of this grammar.
![[FundamentalsCS/Context-freeGrammar/Context-freeGrammar-images/GetImage (6).png]]
With an example
![[FundamentalsCS/Context-freeGrammar/Context-freeGrammar-images/GetImage (7).png]]

Grammar Rules:
![[FundamentalsCS/Context-freeGrammar/Context-freeGrammar-images/GetImage (8).png]]
Sequences/strings:
![[FundamentalsCS/Context-freeGrammar/Context-freeGrammar-images/GetImage (9).png]]
Language:
![[FundamentalsCS/Context-freeGrammar/Context-freeGrammar-images/GetImage (10).png]]


Because grammar is used to describe sequences of characters, we often need to design a grammar based on what we want to represent. 

Remember grammar works recursively! Helps design and derive.
![[FundamentalsCS/Context-freeGrammar/Context-freeGrammar-images/GetImage (11).png]]

Now, since regular language is a subset of context-free languages, we should know how to convert regular expression into context-free grammar.
![[FundamentalsCS/Context-freeGrammar/Context-freeGrammar-images/GetImage (12).png]]
![[FundamentalsCS/Context-freeGrammar/Context-freeGrammar-images/GetImage (13).png]]
![[FundamentalsCS/Context-freeGrammar/Context-freeGrammar-images/GetImage (14).png]]
![[FundamentalsCS/Context-freeGrammar/Context-freeGrammar-images/GetImage (15).png]]
![[FundamentalsCS/Context-freeGrammar/Context-freeGrammar-images/GetImage (16).png]]