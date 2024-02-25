Most objects in game operate in three-dimensional space. And objects are composed of vertex. So, understanding how to express a vertex in mathematical way is the fundamental of  describing the game world with math. 

A vertex at its simplest form carries position value, so it can be treated as a point. We know a point can be express by (Px, Py) if it is using Cartesian Coordinates and resides in 2D space (the x-y space). If it resides in 3D space (the x-y-z space), then we can describe it by (Px, Py, Pz). There are other coordinate systems suitable for game (read GameEngine Page 166), but we focus on Cartesian Coordinates here because it is by far the most common one. 

Cartesian Coordinates in 3D space can be left-handed or right-handed.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage.png]]

Changing one from the other is easy, just reversing the direction of anyone of those axis, not necessarily the z axis, x or y can also work. The difference between them is how to map the numerical representation into 3D space, and both conventions are used for different things. So it is always good to figure it out which convention is applied before working. 

Talking about directions, we need to introduce another mathematical object, vector. A vector is a quantity that has both a magnitude and a direction in n-dimensional space. Its numerical representation using Cartesian Coordinates is the same as point, (Vx, Vy Vz). The difference between a vector and a point is, a point's numerical representation is always relative to the origin of the applied coordinate. A vector is not. A vector is a directional offset relative between two known points, and neither of these points need to be the origin. A vector can be a point when one of those two points is origin, and it is called position vector or displacement vector. Remember a vector also has a direction, so it is more than just an offset. A vector from point A to point B has a direction completely opposite to a vector from point B and point A. But fear not, its numerical representation does show the direction along with the magnitude. 

Because we often use alphabet letter to denote a variable, whether it is a pure number or a vector, we need some ways to tell the difference. The convention is a letter denoting a pure number is written in italics (regular letter) while a letter denoting a vector is written in boldface (bold letter). Pure number used along with vector is often called scalar, as they can be used to scale the vector (changing magnitude). They can also change a vector's direction by being negative number, but this can only change the direction to a completely opposite one. Scalar is just pure number, it can be used normally in algebra, it is just the name got changed to better fit the use case where vector is involved. 

Most programmers and math libraries use the term "vector" to refer both to points (position vectors and vectors in strict linear algebra sense (directional vectors). However, we must understand the difference between point and vector because in some cases, they get treated differently. 

So, we normally treat vertex as a point, and changes applied to vertex are treated as vectors. It is just both have the same numerical representations so that it is easy to get confused.


Vector operations 

Multiplication of a vector by a scalar is accomplished by multiplying the individual components of the vector by the scalar. S here is the scalar, and a is the vector.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (1).png]]
If s = 2, then visually, this operation gives such an effect.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (2).png]]
This scales each component equally, but we can do a non-uniform scale as well, just not with a scalar. We need to use a matrix to perform such a component-wise product, also known as Hadamard product. 

Addition and Subtraction between two vectors 

Addition of two vectors is the addition for each component like this.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (3).png]]

Addition and subtraction is the same thing, subtraction is just adding the negative version of the vector, by that we mean the vector flip its direction through multiplying with a scalar = to -1. Then doing subtraction with scalar (pure number) is what we learn since elementary school. 

Conceptually, we should avoid performing addition between two points although they can be done.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (4).png]]

Getting the magnitude of a vector 

The numerical representation of a vector in Cartesian Coordinates is (Vx, Vy, Vz), this form contains information of both direction and magnitude. The pure magnitude info of a vector is a scalar, and we can get this scalar by converting the numerical representation of the vector like this.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (5).png]]

However, in computer calculation, square root is very expensive, so we often use the squared value instead, getting rid of the square root.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (6).png]]

A vector can get normalization to be a unit vector simply by dividing a vector with its magnitude, like this.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (7).png]]

However, not to get confused about a normalized vector and a normal vector. A normalized vector is an unit vector. And a normal vector is a vector perpendicular to  a surface. 

Multiplications between two vectors 

There are three kinds of multiplications here between two vectors. The dot product produces a scalar. The cross product produces a vector (more accurately a pseudovector). The Hadamard product produces a vector also, but very similar to dot product without the summation. The dot product is easier, so let's discuss it first. Dot product is calculated like this.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (8).png]]
Or using magnitude to calculate the dot product
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (9).png]]
The dot product is commutative (the order doesn't affect the result) and distributive over addition.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (10).png]]

Vector Projection using dot product 

Given a unit vector (magnitude = 1) at any direction, the dot product of another vector and this unit vector represents the length of the projection of the vector onto the infinite line defined by the direction of the unit vector, like this. This scalar value of length is called the scalar projection, and it is equal to the length of the vector projection.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (11).png]]

Dot product is also a good way for testing if two vectors are collinear or perpendicular, or whether they point in roughly the same or roughly opposite directions.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (12).png]]
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (13).png]]

Another usage of dot product is to find the height of a point above or below a plane. We can define a plane with two quantities: a point Q lying anywhere on the plane, and a unit vector n that is perpendicular to the plane. Now, a point P is above the plane. The height is calculated as follow:
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (14).png]]
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (15).png]]
It is using the vector projection idea, but in a 3D sense. 

Now, we move on to the cross product. The cross product of two vectors produces another vector that is perpendicular to those two vectors. And this operation is only defined in 3-dimensional space.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (16).png]]
The calculation goes like this.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (17).png]]
Because the cross product produces a vector, it has a magnitude, and we can get this cross product magnitude from the magnitudes of the two vectors.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (18).png]]
For the direction of the cross product, sure it is perpendicular to both the two vectors. But remember in 3D space, there are still two possibilities based on whether the coordinate system is right-handed or left-handed.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (19).png]]

The cross product is not commutative (order matters).
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (20).png]]
And it is anti-commutative (reverse the order would give a negative result)
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (21).png]]
It is distributive over addition.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (22).png]]
The Cartesian basis vectors (unit vectors at x, y, z axis denoted  as i, j, k respectively.) have following relationships of cross product between each other.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (23).png]]

These relationships define the positive rotation direction about the Cartesian axes. The positive rotations go from x to y (rotation about z axis), from y to z (rotation about x axis), and from z to x (rotation about y axis). It is said that the positive rotation from z to x about y axis is reversed alphabetically, so not from x to z, and that is the reason why the rotation matrix looks inverted as compared to rotation matrix about x and z axis. 

In later study of matrix, we know a 4 by 4 matrix can be used as a way of converting a vector's coordinate system to a different one (still Cartesian Coordinates but the origin and directions of axis are different). This kind of matrix needs the three basis vectors as part of the info. A common way of getting those three basis vectors is first get the k (z axis direction) in object space from many of those ray cast physics, then assuming the j (y axis direction) in object space is the same as the world space, most of the time, they are the same. Then, we can find i(x axis direction) in object space using a cross product of k and j in object space. A similar technique can be used to find a unit vector normal to a surface given three points on the plane (P1, P2, P3). The normal vector n  can be calculated as follow:
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (24).png]]

We mentioned that a cross product yields a Pseudovector, not a regular vector. The subtle difference is when performing a reflection of the coordinate system, like changing a left-handed coordinate system into a right-handed one or vice versa. When this happens, a regular vector transforms into its mirror image, but a pseudovector would transform into its mirror image and change direction. But change to what direction? Need some research. 

Linear interpolation.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (25).png]]

We mentioned matrix before, and now we start studying this subject. 

A matrix is a rectangular array of m*n scalars, and m doesn't need to be equal to n. It is a convenient way of representing linear transformations such as translation, rotation and scale. A example of 3*3 matrix is like this.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (26).png]]

Transformation is linear by following conditions: (need more research)
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (27).png]]

We apply linear transformation to vector through multiplication with matrix. And we normally use 4by4 matrix, because 3by3 matrix can be used to represent rotation and scale in 3D, but not translation. 

An affine matrix is any combination of the atomic transformation matrix representing linear transformation. So, yes, multiple linear transformations can be put together into one matrix through multiplication, that's why it is convenient, the action of putting atomic transformation matrix into one matrix is called concatenation. 

Matrix multiplication goes like this, and it is not commutative (order matters).
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (28).png]]

In order to applied linear transformation to vector through matrix multiplication, we need to first represent vector in the form of matrix. Vector in 3D has three scalar components, and we can either use a row matrix(1*3) or column matrix(3*1) to represent that info.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (29).png]]
V2 is the transpose of V1, and vice versa. Transpose is simply a relationship indicating one matrix is obtained by reflecting the entries of the original matrix across its diagonal. And the T is the symbol.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (30).png]]
Why do we need transpose? Well, mainly because V1 and V2 can be used to represent the same vector, but they are not the same in strict matrix sense. When we do matrix multiplication, we need to make sure we convert our vector into the correct form of matrix, and people are using both row matrix and column matrix, the easiest way to convert them is through transpose.

One important thing to remember is two matrices can be multiplicated together only when the inner dimensions of the two matrices are equal.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (31).png]]

So if multiple transformation matrices A, B, C are applied in order to vector v, the way we write and read them is different based on whether vector v is represented as a row matrix or column matrix.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (32).png]]

Identity matrix is a matrix that when multiplied by any other matrix, yields the very same matrix. It is denoted by symbol I and it is always a square matrix with 1's along the diagonal and 0's everywhere else. Here is a identity matrix in its 3by3 form.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (33).png]]

Why do we need identity matrix? Well, the inverse of a matrix A is another matrix (denoted A^-1) that undoes the effects of matrix A. And the multiplication between a matrix and its own inverse always yields the identity matrix. Not all matrices have inverses, but all affine matrices do have inverse because when you think about it, a linear transformation can always be undone by the opposite linear transformation. The way we find the inverse of a matrix is through Gaussian elimination or lower-upper decomposition, if one exists. The inverse of an pure rotation matrix is exactly equal to its transpose, which is good because transpose is easy to get. 

Also, be careful with the order when getting the inverse of a concatenated matrix. It makes sense if you apply A, B, C in order, then when you cancel the effect, you will begin from the last one first.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (34).png]]
This reverse order also happens when we get transpose of a concatenated matrix.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (35).png]]

Homogeneous Coordinates 

Remember we mentioned 4by4 matrix can be use to represent all linear transformations. Well, a vector in 3D space contains only three components, in order to be able to multiply with a 4by4 matrix, it needs one more component. We extend a point or vector from three dimensions to four in a way where we set the fourth component (usually denoted by w) to 1. This vector then is written in homogeneous coordinates. 

Conceptually, a point can be applied with translation, rotation, and scale. However, a directional vector can be applied only with rotation and scale. It can be applied with translation, but this would alter its magnitude, as a result, we break this vector by turning it into something else. So the translation effect is ignored when we applied a multiplication of matrix, by setting the w component equal to 0. 

When we are done with multiplication, we can convert vectors or points back to three-dimensional space through this. Or we can just directly ignore the fourth component.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (36).png]]

Now we look at the 4by4 matrix.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (37).png]]
Because a 3by3 matrix can represent rotation and scale, so the U part is for rotation and scale values. The t part is for translation value. And the rest (the fourth column) is always the same with 3 zeros and 1 one at the bottom. 

Pure translation matrix multiplication goes like this. Identity matrix in U part yields no rotation and no scale.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (38).png]]
Pure rotation matrix multiplication goes like this. All zeros for t part yields no translation.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (39).png]]
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (40).png]]
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (41).png]]
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (42).png]]
If Sx = Sy = Sz, then we call this uniform scale. And when a uniform scale matrix is concatenated with a rotation matrix, the order doesn't matter. 

Because the rightmost column of an affine 4by4 matrix is always the same, many people often omit the fourth column to save memory, using a 4by3 matrix.


Coordinate space. 

In game, we often use Cartesian Coordinates, and changing the coordinate space means changing the origin and possibly the direction of the axis, but still using Cartesian Coordinates. 

There are three common coordinate spaces in game, model space, world space, and view space. 

Model space use the center or a explicitly set point of the model as the origin. When modifying vertices belong to this mesh, we often use model space. World space use the origin of the whole scene as the origin of the coordinate space. View space (camera space) use the focal point of a camera to be the origin. The world space is the ultimate parent space, and any other spaces is the child space underneath it. 

We often need to change coordinate space for calculation of vertex across different meshes. Matrix multiplication can again represent the conversion between space, matrix and its multiplication is so important in game.
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (43).png]]
![[GameEngine/3DMathForGame/3DMathForGame-images/GetImage (44).png]]
tC is the position of the origin in child space, but in parent space value. iC, jC, kC are all in parent space value. 

Quaternions