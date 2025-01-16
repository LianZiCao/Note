表面（surface，不要与“物体表面”混淆）这一术语表示显存（尽管表面也可位于系统内存，但这里通常指代的是显存端）

## 组件对象模型
要获取指向某COM接口的指针，需借助特定函数或另一COM接口的方法。  
另外COM对象会统计其引用计数；因此，在使用完某接口时，我们便应调用它的Release方法。  
为了辅助用户管理COM对象的生命周期，WIndows运行时库（Windows Runtime Library，WRL）专门为此提供了Microsoft::WRL::Comptr类（#include<wrl.h>），可作为COM对象的智能指针使用。常用的三个Comptr方法
![图片](Chapter4/1.png)  
COM接口都以大写字母“I”作为开头

## 纹理格式
2D纹理（2D texture）是一种由数据元素构成的矩阵（可将此“矩阵”看作2D数组）可用作存储2D图像数据（这种情况下，纹理中的每个元素存储的都是一个像素的颜色）  
1D,2D,3D纹理就相当于特定数据元素所构成的1D,2D,3D数组  
并不是任意类型的数据元素都能用于组成纹理，只能存储[DXGI_FORMAT](https://learn.microsoft.com/en-us/windows/win32/api/dxgiformat/ne-dxgiformat-dxgi_format)枚举类型中描述的特定格式的数据元素