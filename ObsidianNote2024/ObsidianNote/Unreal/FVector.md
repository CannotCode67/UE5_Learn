
`FVector(x, y, z)`
API above is the constructor of a FVector containing three elements. This is like the Vector3 class in Unity.

`FVector::Dist(FVector1, FVector2)`
API above can get the distance between two positions represented by passing in vectors. It works for vector3. Also, FVector is probably the class to look into if we are trying to find APIs related to vector calculation and manipulation.

`FVector(x, y, z).GetSafeNormal()`
API above returns the normalized version of the FVector. If the vector length is too small to normalize, it returns (0, 0, 0). This is like Vector3.Normalize() in Unity.

`FVector.ToCompactString()`
API above returns the FVector's value in FString type, it is an explicit conversion from FVector to FString. Simialr methods should be available for other built-in types as well.