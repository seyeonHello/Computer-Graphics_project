# WebGL Tutorial

## 0. Motivation
In class I was confused about fragment operation and rotation on several axes. 
So I wanted to make a tutorial on transformation and fragment operation. 
I hope this tutorial will help people who are having difficulties like me.

## 1. Outline
- Texture Mapping
- Transformation
- Fragment Operation

## 2. File structure
- box.html
- box.js
- gl-matrix.js
- shape.jpg

## 3. Basic information
- Canvas size : width="600" height="450"
- Vertex shader code
```c
var vertexShaderSource = '\
			attribute highp vec4 myVertex; \
			attribute highp vec4 myColor; \
			attribute highp vec2 myUV; \
			uniform mediump mat4 mMat; \
			uniform mediump mat4 vMat; \
			uniform mediump mat4 pMat; \
			varying  highp vec4 color;\
			varying mediump vec2 texCoord;\
			void main(void)  \
			{ \
				gl_Position = pMat * vMat * mMat * myVertex; \
				gl_PointSize = 8.0; \
				color = myColor; \
				texCoord = myUV*1.0; \
			}';
```
- Fragment Shader code
```c
var fragmentShaderSource = '\
			varying highp vec4 color; \
			varying mediump vec2 texCoord;\
			uniform sampler2D sampler2d;\
			void main(void) \
			{ \
				gl_FragColor = 0.0 * color + 1.0 * texture2D(sampler2d, texCoord); \
                gl_FragColor = pow(gl_FragColor,vec4(1.0,1.0,1.0,1.0)); \
                gl_FragColor.a=1.5; \
			}';
```
## 4. Function
- Texture Mapping <br>
You can make cube with texture.<br>
In the vertex shader, you must modify the texture coordinates to read. You must also modify the Fragment Shader.(Check the code above) <br>
```c
var texture = gl.createTexture();
gl.bindTexture(gl.TEXTURE_2D, texture);

var image = new Image();
image.src = "please write image src"

image.addEventListener('load', function() {
		gl.bindTexture(gl.TEXTURE_2D, texture);
		gl.texImage2D(gl.TEXTURE_2D, 0, gl.RGBA, gl.RGBA,gl.UNSIGNED_BYTE, image);
		gl.generateMipmap(gl.TEXTURE_2D);
		});
```
- Rotation <br>
You can rotate the cube on three axes using model matrix in the rotate function. <br>
Tip) if you want to rotate the both axes, write two codes at the same time. <br>
```c
mat4.rotateX(mMat, mMat, rotX);
```
```c
mat4.rotateY(mMat, mMat, rotY);
```
```c
mat4.rotateZ(mMat, mMat, rotZ);
```

- Translation <br>
In the translate function, you can move the cube using a model matrix to the x-axis, y-axis, and z-axis respectively. <br>
```c
mat4.translate(mMat, mMat, [x_position,y_position,z_position]);
```
- Scale <br>
You can enlarge or reduce the size of the cube using model matrix in the scale function. <br>
Tip) the larger the scaleSize, the larger the cube. <br>
```c
mat4.scale(mMat, mMat,[scaleSizeX, scaleSizeY, scaleSizeZ]);
```
- Reflection <br>
You can reflect the cube. <br>
Reflection corresponds to negative scale facotrs. <br>
```c
function xReflection(){
  scaleSizeX=-scaleSizeX;
}
function yReflection(){
  scaleSizeY=-scaleSizeY;
}
function zReflection(){
  scaleSizeZ=-scaleSizeZ;
}
```

- Scissor Test <br>
You can take a scissor test, one of the Fragment operations. <br>
The scissor test is a function that prints only specific areas specified on the screen. <br>
Tip) you need to set the size to cut. <br>
```c
gl.clear(gl.COLOR_BUFFER_BIT|gl.STENCIL_BUFFER_BIT);
gl.enable(gl.SCISSOR_TEST);
gl.scissor(200, 130, 200, 200);
```
If you want to turn off the scissors test,
```c
gl.disable(gl.SCISSOR_TEST);
```
- Depth Test <br>
You can take a dept test, one of the Fragment operations. <br>
Depth test is disabled by default. In order to activate the depth test, You should activate the gl.DEPTH_TEST option. <br>
If you activate the Depth test, the back doesn't show up in front. <br>
```c
gl.enable(gl.DEPTH_TEST);
```
If you want to turn off the depth test,
```c
gl.disable(gl.DEPTH_TEST);
```
## Copyright
(CC-NC-BY) Kim Seyeon 2020 <br>
A picture drawn by me
## References
https://git.ajou.ac.kr/hwan/cg_course/-/blob/master/WebGL/frag_op/hello.js <br> 
https://git.ajou.ac.kr/hwan/webgl-tutorial/-/tree/master/student2018/201320968


