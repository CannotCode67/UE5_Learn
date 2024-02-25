
under sequence
if one child node has the abort self decarator, then while running this child node's task, the abort condition is triggered, it will not run its remaining task. Also, any child node behind will not run. As if it returns a fail to the sequence, and sequence will not continue.

1. needs to sense the target -> pick up by the ai perception system
2. check the target distance constantly -> BTS
3. bind and unbind certain events to prepare triggeration of actions -> BTS
4. chech the target angle.
5. close up on the target -> move to node, the accepting radius should be set to accommodate the cloest-range attack.
6. Now, each attack montage should be put under its own sequence, and those sequences should check if target distance is within the attack range for the montages. Only so, the sequences get to be executed. 
7. For openr, it is good to use a forward-moving attack to close up the last distance while attacking. Such as spin slash. If the forward-moving attack is fast, then we should not set focus. However, if it is slow like combo slash, we should set focus, as the character moves forward a bit with every slash. Fixing the rotation while doing that is hardly noticeable.
8. 
9. 