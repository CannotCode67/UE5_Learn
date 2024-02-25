One of the most useful nodes is blend node. It combines the foreground input and the background input, and it has blending mode parameter to define how this two inputs should be blended. Also, an opacity input to work as a mask, white means execute the blending at 100%, black means not execute the blending, any value in between means doing it at relative %.

To use this node correctly, we need to understand the blending modes.

For greyscale value, black is 0, white is 1. For height map, black is inward, white is outward.

Copy - treat two inputs as if the foreground is stick on top of the background, it would be useless without the opacity input or mask for it just shows the foreground. It works for color maps and greyscale maps.

Add - add the greyscale value of the same spots on two inputs, if the sum is bigger or equal to the upper limit(1), then it stays at 1. It only works for greyscale map?

Subtract - greyscale value on background subtracts the greyscale value on foreground, if the result is smaller or equal to the lower limit(0), then it stays at 0. It only works for greyscale map?

Multiply - greyscale value on one input times the greyscale value on the other input. Since the biggest value is 1, this would only result an equal or smaller value. It could retain the relative values on one input(aka reserve the pattern) if the other input has a consistent greyscale values because they are multiplying the same number.

AddSub - background would add or substract or do nothing based on the greyscale value on foreground. If the greyscale value is bigger than 0.5, then add, if it is smaller than 0.5 then substract. If it is equal to 0.5, then do nothing. The result is clamped between the upper limit(1) and the lower limit(0).

Max - compares the greyscale values of the same spots on two inputs, the bigger one would take the spot.

Min - compares the greyscale values of the same spots on two inputs, the smaller one would take the spot.

Divide - don’t understand yet.

Overlay - background would screen or multiply or do nothing based on the greyscale value on foreground. If the greyscale value is bigger than 0.5, then screen, if it is smaller than 0.5, then multiply. If it is 0.5, then do nothing.

Screen - With Screen blend mode the values of the pixels in the two inputs are inverted, multiplied, and then inverted again. The result is the opposite effect to multiply and is always equal or higher (brighter) compared to the original.