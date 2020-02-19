# 运行：
bin文件夹中的exe可以直接运行，打开工程文件后，可在x64平台下进行Debug。所有的需要读取的文件都设置成了相对路径，且对于Debug和Release进行了区分，所以应该是可以直接用的。32位平台应该是不行的，为了压缩包体积，删掉了32位的Lib。
# 作业内容：
* 1、首先打开程序是自由视角的相机，可以使用W、A、S、D、Z、空格控制摄像机进行前、左、后、右、下、上的移动，按住鼠标右键进行拖动可以改变相机的朝向；<br>
* 2、按"G"可以在相机的自由视角模式与第一人称模式之间切换，进入第一人称视角后，可以使用W、A、S、D控制车辆进行前、左、后、右的移动，鼠标的移动可以控制第一人称视角；<br>
* 3、按"F"可以在相机的第一人称模式与第三人称之间进行切换，进入第三人称视角后，同样可以使用W、A、S、D控制车辆进行前、左、后、右的移动，鼠标的移动会使视角围绕车辆旋转;<br>
* 4、按"C"可以使一个灯泡移动到相机当前的位置，且具有和相机一样的朝向，这个作业中实现了环境光、点光源和光照损失;<br>
* 5、车辆由一个立方体和四个圆柱体构成，圆柱体和立方体的相对位置保持不变，会跟随车身即立方体的旋转，在车前进时车轮自身也会有旋转。<br>
# 源码：
* 1、所使用的库有<d3d11.h>、<wrl/client.h>(主要用于ComPtr类的使用)、<DirectXMath.h>、<d3dcompiler.h>、<comdef.h>(对HRESULT进行解释以输出错误信息)、<Windows.h>；一些读取资源的类<SpriteBatch.h>、<SpriteFont.h>(读取字体并输出文字)、<WICTextureLoader.h>、<assimp>(开源库，用于读取模型文件)；<br>
* 2、程序入口文件是Source.cpp,消息处理经过抽象后在Engine类中处理，图像相关的代码主要在Graphic类中处理，包括DirectX、shader和Scene的初始化，每一帧的图像绘制在Graphics::RenderFrame()中处理。<br>
* 3、窗口消息被分发到WindowContainer::WindowProc()函数中，消息被封装成KeyboardEvent和MouseEvent，从而可以直接被Engine类处理。<br>
* 4、将物体的顶点和Textures等自身属性抽象成model类，将物体与世界的交互所需的位置、旋转等属性抽象成GameObject类，对于3D对象，需要有额外的方向向量，因此有了继承GameObject类的GameObject3D类，由于物体需要在每一帧进行位置或者旋转的更新，因此该类设有虚基类virtual void UpdateMatrix() = 0，因此有了继承GameObject3D的RenderableGameObject类,该类实现了UpdateMatrix()，并持有model类。本程序中所有的3D对象都继承自RenderableGameObject类，并依据自身的需求进行扩展;<br>
* 5、shader的Shader Model版本是5.0，根据对象使用不同的vertex shader和pixel shader;顶点的输入包括position,texcoord和normal。<br>
* 6、其他的根据名字应该能看出作用了。<br>
# 写一些感悟：
还有很多东西没时间做，内容上比如阴影，车轮左右转向，物理手感这些进阶的内容，框架上读取模型的类很不完善，实例化（一个模型可以多次绘制）没做，代码的组织还可以再改进一下，还有所有的运动其实都应该在固定间隔帧中进行（类似于unity中的update和fixedupdate）等等。实在是毕业论文比较急..不过这次作业也是我第一次写这么多代码，用到了很多C++11的特性，也学到了项目的合理组织，3D图像的基础和绘制过程，收获很大。
