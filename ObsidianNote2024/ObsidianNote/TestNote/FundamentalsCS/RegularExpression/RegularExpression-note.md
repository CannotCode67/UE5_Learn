What is a regular expression conceptually? 

A regular expression is a sequence of characters that define a search pattern (languages). 

Its link to finite automata 

American mathematician Stephen Cole Kleene formalized the description of a regular language, and his theorem states a language is regular if and only if it can be described by a regular expression. 

In other words: 

1.  If a language is described by a regular expression, then it is regular. 
    
2.  If a language is regular, it can be described by a regular expression.
 ![[FundamentalsCS/RegularExpression/RegularExpression-images/GetImage.png]]
 In the end, we are interested in languages (the set of all strings accepted by finite autoamata), and regular expression is just a tool to help us precisely describe the languages, for the set might go infinite but we still need to describe it acuratly. 

Question: language of finite automata has to be regular? Seems yes. 

What can be a regular expression?![[FundamentalsCS/RegularExpression/RegularExpression-images/GetImage (1).png]]

Atomic regular expression is the simplest unit of regular expression. 

we can add them up, but we still get a simple regular expression, like: 

aaa is  a regular expression and its lanuage is {aaa}. 

Compound regular expression is ones with regular operations. 

There are three kinds of regular operations.
![[FundamentalsCS/RegularExpression/RegularExpression-images/GetImage (2).png]]
![[FundamentalsCS/RegularExpression/RegularExpression-images/GetImage (3).png]]
![[FundamentalsCS/RegularExpression/RegularExpression-images/GetImage (4).png]]![[FundamentalsCS/RegularExpression/RegularExpression-images/GetImage (5).png]]
![[FundamentalsCS/RegularExpression/RegularExpression-images/GetImage (6).png]]
Remember concatenation cares about order, so A concatenate B is totally different from B concatenate A. 

Remember back in automata,
![[FundamentalsCS/RegularExpression/RegularExpression-images/GetImage (7).png]]
This star and plus sign is actually a regular operatio, thus automata and regular expression are closely related. 

Converting finite automata to regular expression, watch 5.201&5.203 videos. 

Examples:
![[FundamentalsCS/RegularExpression/RegularExpression-images/GetImage (8).png]]
![[FundamentalsCS/RegularExpression/RegularExpression-images/GetImage (9).png]]
Working backwards, Given language and work out the regular expression
![[FundamentalsCS/RegularExpression/RegularExpression-images/GetImage (10).png]]
![[FundamentalsCS/RegularExpression/RegularExpression-images/GetImage (11).png]]
![[FundamentalsCS/RegularExpression/RegularExpression-images/GetImage (12).png]]
![[FundamentalsCS/RegularExpression/RegularExpression-images/GetImage (13).png]]
![[FundamentalsCS/RegularExpression/RegularExpression-images/GetImage (14).png]]
