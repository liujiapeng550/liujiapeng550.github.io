1. shader代码可以用文本文件保存，可以在程序中用字符串保存，但最终还是必须在程序中以字符串的形式传入Shader对象中；
2. 创建Shader对象（glCreateShader——成功时返回非0的无符号Handle值，指涉Shader对象）；
3. 把shader代码传入shader对象（glShaderSource——注意此函数的参数，字符流地址参量是GLchar*，不支持宽字符。执行后可删除内存上保存的shader代码字符串副本）；
4. 编译Shader对象——正如我们编译任何代码一样（glCompileShader——应该以GL_COMPILE_STATUS为参调用glGetShaderiv检查编译是否成功。如果代码出现问题会在这个阶段报错，debug时可用glGetError查看更具体的错误类型）；
5. 按上步骤把一个“过程”中需要的各类型shader编译好（通常每种类型最多建一个shader对象——因为对于一个渲染管道（流程）来说，顶点、单几何体、像素都只处理一遍，不可能返回。shaders只是插入这个流程中取代对应的固定管道处理的“外挂”[在抹除固定管道处理、全shader时代来临之前可以这么说]，在一个渲染流程中起作用的shaders姑且统称为一个shader过程）；
6. OpenGL通过一个名为shaderProgram的对象来与shader交互。也可以说shaders通过这个对象连接到我们的OpenGL应用程序中，它具体地指涉shader过程。宏观概念上类似于我们平时写的“程序”，不过它是基于GPU的；
7. 创建这么一个shaderProgram对象（glCreateProgram——成功时返回非0的无符号Handle值，指涉Shader程序对象）；