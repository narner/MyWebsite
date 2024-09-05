---
title: "Working with Transformation Matrices in Swift"
thumbnail: /blog_assets/2024/MatrixTestApp.png
date: "2024-09-05"
---




As part of some recent work, I was working on implementing applying transforms to 3D objects in a Swift app. I was having a really difficult time gaining an intuitive understanding of how the matrices behaved. 

I had Claude help me generate a test app to visualize how modifying position, scale, and rotation values would affect the resulting matrices being used to modify the blue 3D cube.

![Matrix Test App](/blog_assets/2024/MatrixTestApp.png)

This helped me to gain a better understanding of how the matrices behaved. I had previously assumed that there would be a one-to-one mapping 

The code for this app can be found here: [narner/3D-Matrix-Operation-Visualizer: Real-time 3D object transformation demo for Swift](https://github.com/narner/3D-Matrix-Operation-Visualizer)



## Matrix Representation

A transformation matrix in 3D is typically a 4x4 matrix, which allows for the representation of both linear transformations (like rotation and scaling) and translations (movement in space). The general form of a 4x4 transformation matrix is:	

<table>
  <tr>
    <td>|</td><td>m11</td><td>m12</td><td>m13</td><td>m14</td><td>|</td>
  </tr>
  <tr>
    <td>|</td><td>m21</td><td>m22</td><td>m23</td><td>m24</td><td>|</td>
  </tr>
  <tr>
   <td>|</td> <td>m31</td><td>m32</td><td>m33</td><td>m34</td><td>|</td>
  </tr>
  <tr>
   <td>|</td> <td>0</td><td>0</td><td>0</td><td>1</td><td>|</td>
  </tr>
</table>

&nbsp;



The **top-left 3x3 submatrix** (m11 to m33) represents the rotation and scaling of the object.

<table>
  <tr>
    <td>|</td><td><b>m11</b></td><td><b>m12</b></td><td><b>m13</b></td><td>m14</td><td>|</td>
  </tr>
  <tr>
    <td>|</td><td><b>m21</b></td><td><b>m22</b></td><td><b>m23</b></td><td>m24</td><td>|</td>
  </tr>
  <tr>
   <td>|</td> <td><b>m31</b></td><td><b>m32</b></td><td><b>m33</b></td><td>m34</td><td>|</td>
  </tr>
  <tr>
   <td>|</td> <td>0</td><td>0</td><td>0</td><td>1</td><td>|</td>
  </tr>
</table>
&nbsp;

&nbsp;

The **fourth column** (m14, m24, m34) represents the translation (position) of the object in 3D space.

<table>
  <tr>
    <td>|</td><td>m11</td><td>m12</td><td>m13</td><td><b>m14</b></td><td>|</td>
  </tr>
  <tr>
    <td>|</td><td>m21</td><td>m22</td><td>m23</td><td><b>m24</td><td>|</td>
  </tr>
  <tr>
   <td>|</td> <td>m31</td><td>m32</td><td>m33</td><td><b>m34</td><td>|</td>
  </tr>
  <tr>
   <td>|</td> <td>0</td><td>0</td><td>0</td><td>1</b></td><td>|</td>
  </tr>
</table>
&nbsp;

&nbsp;

The **bottom row** is typically [0, 0, 0, 1], which is used to maintain the properties of homogeneous coordinates.

<table>
  <tr>
    <td>|</td><td>m11</td><td>m12</td><td>m13</td><td>m14</td><td>|</td>
  </tr>
  <tr>
    <td>|</td><td>m21</td><td>m22</td><td>m23</td><td>m24</td><td>|</td>
  </tr>
  <tr>
   <td>|</td> <td>m31</td><td>m32</td><td>m33</td><td>m34</td><td>|</td>
  </tr>
  <tr>
   <td>|</td> <td><b>0</b></td><td><b>0</b></td><td><b>0</b></td><td><b>1</b></td><td>|</td>
  </tr>
</table>

&nbsp;



We’ll look at ways to extract specific properties of the transformation matrix from code in `simd_float4x4+Extension.swift`.



## Transformation Types

**Translation**: These are used to move an object from one location to another, and can be represented as:

<table>
  <tr>
    <td>|</td><td>m11</td><td>m12</td><td>m13</td><td>m14</td><td>|</td>
  </tr>
  <tr>
    <td>|</td><td>m21</td><td>m22</td><td>m23</td><td>m24</td><td>|</td>
  </tr>
  <tr>
   <td>|</td> <td>m31</td><td>m32</td><td>m33</td><td>m34</td><td>|</td>
  </tr>
  <tr>
   <td>|</td> <td><b>tx</b></td><td><b>ty</b></td><td><b>tz</b></td><td>1</td><td>|</td>
  </tr>
</table>

&nbsp;

Where (**tx, ty, tz**) are the translation distances along the x, y, and z axes. This can be represented in Swift like so:

```
var position: SIMD3<Float> {
    .init(columns.3.x, columns.3.y, columns.3.z)
}
```

&nbsp;

**Scaling**: This changes the size of an object. The scaling matrix is defined as:

<table>
  <tr>
    <td>|</td><td><b>sx</b></td><td><b>sy</b></td><td><b>sz</b></td><td>m14</td><td>|</td>
  </tr>
  <tr>
    <td>|</td><td>m21</td><td>m22</td><td>m23</td><td>m24</td><td>|</td>
  </tr>
  <tr>
   <td>|</td> <td>m31</td><td>m32</td><td>m33</td><td>m34</td><td>|</td>
  </tr>
  <tr>
   <td>|</td> <td>tx</td><td>ty</td><td>tz</td><td>1</td><td>|</td>
  </tr>
</table>

&nbsp;&nbsp;

Where (**sx, sy, sz**) are the scaling factors along the respective axes.

``` 
var scale: SIMD3<Float> {
	.init(
		simd_length(SIMD3<Float>(columns.0.x, columns.0.y, columns.0.z)),
		simd_length(SIMD3<Float>(columns.1.x, columns.1.y, columns.1.z)),
		simd_length(SIMD3<Float>(columns.2.x, columns.2.y, columns.2.z))
	)
}
```

&nbsp;

**Rotation**: The rotation matrices are used to rotate an object around the X, Y, and Z axes; and can be represented as:



**Rotation around X-axis**:

<table>
  <tr>
    <td>|</td><td>1</td><td>0</td><td>1</td><td>0</td><td>|</td>
  </tr>
  <tr>
    <td>|</td><td>0</td><td><b>cos(θ)</b></td><td><b>-sin(θ)</b></td><td>0</td><td>|</td>
  </tr>
  <tr>
    <td>|</td><td>0</td><td><b>sin(θ)</b></td><td><b>cos(θ)</b></td><td>0</td><td>|</td>
  </tr>
  <tr>
    <td>|</td><td>0</td><td>0</td><td>0</td><td>1</td><td>|</td>
  </tr>
</table>
&nbsp;



<p><b>Rotation around Y-axis</b>:</p>
<table>
  <tr>
    <td>|</td><td><b>cos(θ)</b></td><td>0</td><td><b>sin(θ)</b></td><td>0</td><td>|</td>
  </tr>
  <tr>
    <td>|</td><td>0</td><td>1</td><td>0</td><td>0</td><td>|</td>
  </tr>
  <tr>
    <td>|</td><td><b>-sin(θ)</b></td><td>0</td><td><b>cos(θ)</b></td><td>0</td><td>|</td>
  </tr>
  <tr>
    <td>|</td><td>0</td><td>0</td><td>0</td><td>1</td><td>|</td>
  </tr>
</table>
&nbsp;



**Rotation around Z-axis**:

<table>
  <tr>
    <td>|</td><td><b>cos(θ)</b></td><td><b>-sin(θ)</b></td><td>0</td><td>0</td><td>|</td>
  </tr>
  <tr>
    <td>|</td><td><b>sin(θ)</b></td><td><b>cos(θ)</b></td><td>0</td><td>0</td><td>|</td>
  </tr>
  <tr>
    <td>|</td><td>0</td><td>0</td><td>1</td><td>0</td><td>|</td>
  </tr>
  <tr>
    <td>|</td><td>0</td><td>0</td><td>0</td><td>1</td><td>|</td>
  </tr>
</table>
&nbsp;



This can be represented in Swift like so:

```
var rotationMatrix: simd_float3x3 {
	let scale = self.scale
		return simd_float3x3(
		SIMD3<Float>(columns.0.x, columns.0.y, columns.0.z) / scale.x,
		SIMD3<Float>(columns.1.x, columns.1.y, columns.1.z) / scale.y,
		SIMD3<Float>(columns.2.x, columns.2.y, columns.2.z) / scale.z
	)
}
```



## Matrix Creation

There are a couple of ways to create Transformation Matrix from scratch. The init function below takes a `SIMD3<Float>` of Euler angles as input, and constructs a matrix out of them.

```
init(rotationZYX eulerAngles: SIMD3<Float>) {
	// Convert degrees to radians
	let radiansX = eulerAngles.x * Float.pi / 180
	let radiansY = eulerAngles.y * Float.pi / 180
	let radiansZ = eulerAngles.z * Float.pi / 180

	let cx = cos(radiansX), sx = sin(radiansX)
	let cy = cos(radiansY), sy = sin(radiansY)
	let cz = cos(radiansZ), sz = sin(radiansZ)

	let rotationMatrix = simd_float3x3(
		SIMD3<Float>(cy * cz, cy * sz, -sy),
		SIMD3<Float>(sx * sy * cz - cx * sz, sx * sy * sz + cx * cz, sx * cy),
		SIMD3<Float>(cx * sy * cz + sx * sz, cx * sy * sz - sx * cz, cx * cy)
	)

	self.init(rotationMatrix)
 }
```


There are a couple of parts here worth looking at. First are [Euler Angles](https://en.wikipedia.org/wiki/Euler_angles). Euler Angles represent 3 different angles that describe rotation of an object around an axis. These angles are known as: 

* X-Axis (Roll)

* Y-Axis (Pitch)

* Z-Axis (Yaw)

  


![RollPitchYaw](/blog_assets/2024/RollPitchYaw.png)


The second thing is the conversion from Radians to Degrees. A Degree is 1/360th of a full rotation around a circle. A radian is the angle formed when the arc length equals the radius of a circle. One full circle is about 6.28 (2π) radians.


![RadiansToDegrees](/blog_assets/2024/RadiansToDegrees.png)

You could construct the matrix directly from radians, however, degrees are much more common in user-facing applications for the reason that they are easier to reason about than radians. 

![DegreesAndRadians](/blog_assets/2024/DegreesAndRadians.png)



Additionally, converting the Euler angles to degrees prior to creating the transform matrix allows for a visually smoother experience.

You can see that here: the first video shows what it looks like when we adjust the rotation of the cube in radians, and the second when we adjust it in degrees. 

[![Radians](http://img.youtube.com/vi/AKQ2WSV8wSM/0.jpg)](https://youtu.be/AKQ2WSV8wSM"")



[![Degree](http://img.youtube.com/vi/AKQ2WSV8wSM/0.jpg)](https://youtu.be/AKQ2WSV8wSM"")



## Final Thoughts

Understanding transformation matrices is crucial for Swift developers working in 3D graphics, games, or AR. By breaking down the 4x4 matrix components and visualizing their effects, we can understand how modifying transformation matrices can affect the scaling, position, and rotation of 3D objects. 

[![](http://img.youtube.com/vi/2wgWCECdESg/0.jpg)](https://youtu.be/2wgWCECdESg"")



