更多与游戏相关的数学知识  
[Verth04] [Lengyel02]

配置属性 → C/C++  → 语言 → “符合模式”（Conformance Mode）指编译器如何处理 C++ 代码时，是否严格遵循 C++ 标准。  

用较多的参考系：记录向量在每一种参考系中的对应坐标

Direct3D采用左手坐标系

![图片](Chapter1\1.png)

同维向量之间才可以加法运算

![图片](Chapter1\2.png)
![图片](Chapter1\3.png)

把一个向量的长度变为单位长度称为向量的**规范化**

![图片](Chapter1\4.png)

**点积**/数量积/内积/**标量积** 计算结果为标量值的向量乘法运算

![图片](Chapter1\5.png)

余弦定理有  
![图片](Chapter1\6.png)
![图片](Chapter1\7.png)

![图片](Chapter1\8.png)
![图片](Chapter1\9.png)
(**n**是单位向量)  
称**p**为向量**v**落在向量**n**上的正交投影（orthogonal projection）
![图片](Chapter1\10.png)
![图片](Chapter1\11.png)

若**n**不具有单位长度，规范化有
![图片](Chapter1\12.png)

![图片](Chapter1\13.png)


由于数值处理中的数值精度问题，规范正交集随之逐步变成非规范正交集，因此要用正交化

对2D情况
![图片](Chapter1\14.png)
![图片](Chapter1\15.png)
![图片](Chapter1\16.png)
![图片](Chapter1\17.png)
![图片](Chapter1\18.png)  
  
  
  
对3D情况
![图片](Chapter1\19.png)
![图片](Chapter1\20.png)
![图片](Chapter1\21.png)  
![图片](Chapter1\22.png)
![图片](Chapter1\23.png)
![图片](Chapter1\24.png)  
  
  
  
通用情况
![图片](Chapter1\25.png)

叉积/向量积/外积 计算结果也为向量
![图片](Chapter1\26.png)
![图片](Chapter1\27.png)
![图片](Chapter1\28.png)

2D向量的伪叉积  
![图片](Chapter1\29.png)  
![图片](Chapter1\30.png)

对于3D情况还有另外一种正交规范策略
![图片](Chapter1\31.png)

向量**w1**和**w2**可以不同与向量**v1**和**v2**，不想要改变方向的向量放**w0**

处于标准位置的向量能表示出3D空间中的特定位置，称其为**位置向量**

DirectXMath是为Direct3D应用程序的3D数学库
采用SIMD流指令扩展2（Streaming SIMD Extension 2,SSE2）  
单指令多数据 （Single Instruction Multiple Data,SIMD）  
使用DirectXMath库，添加头文件`#include<DirectXMath.h>`  
命名空间`DirectX`  
一些相关的数据，添加头文件`#include<DirectXPackedVector.h>`  
命名空间`DirectX::PackedVector`

![图片](Chapter1\32.png)

字节对齐准则  
结构体变量的首地址能够被其对齐字节数大小所整除。  
结构体每个成员相对结构体首地址的偏移都是成员大小的整数倍，如不满足，对前一个成员填充字节以满足。  
结构体的总大小为结构体对最大成员大小的整数倍，如不满足，最后填充字节以满足。  

对XMVECTOR，用不到向量分量的设置为0
![图片](Chapter1\33.png)
![图片](Chapter1\34.png)
![图片](Chapter1\35.png)
![图片](Chapter1\36.png)

为了提高效率，可以将`XMVECTIR`类型的值作为函数的参数，直接传送至SSE/SSE2寄存器里，而不存在栈内。因此为了使代码更具通用性，不受具体平台，编译器的影响，我们将利用`FXMVECTIR`、`GXMVECTOR`、`HXMVECTIR`和`CXMVECTOR`类型来传递`XMVECTIR`类型的函数。基于特定的平台和编译器，它们会自动被定义为适当的类型。另外一定要把**约定注解**`XM_CALLCONV`加载函数名之前，它会根据编译器的版本确定出对应的约定属性。

![图片](Chapter1\37.png)
![图片](Chapter1\38.png)
构造函数前三个XMVECTOR用FXMVECTOR且不用XM_CALLCONV

![图片](Chapter1\39.png)
![图片](Chapter1\40.png)

XMVECTOR类型的常量实例应该用XMVECTORF32类型来表示
基本上在运用初始化语法的时候就要使用XMVECTORF32类型
也可以通过XMVECTORU32类型来创建由整型数据构成的XMVECTOR常向量

![图片](Chapter1\41.png)
![图片](Chapter1\42.png)
![图片](Chapter1\43.png)
![图片](Chapter1\44.png)
![图片](Chapter1\45.png)
![图片](Chapter1\46.png)
![图片](Chapter1\47.png)

![图片](Chapter1\48.png)
![图片](Chapter1\49.png)
![图片](Chapter1\50.png)

![图片](Chapter1\51.png)

[XMVectorAbs function (directxmath.h)](https://learn.microsoft.com/en-us/windows/win32/api/directxmath/nf-directxmath-xmvectorabs)  
Computes the absolute value of each component of an XMVECTOR.
```C+
XMVECTOR XM_CALLCONV XMVectorAbs(
  [in] FXMVECTOR V
) noexcept;
```

[XMVectorCos function (directxmath.h)](https://learn.microsoft.com/en-us/windows/win32/api/directxmath/nf-directxmath-xmvectorcos)  
Computes the cosine of each component of an XMVECTOR.
```C+
XMVECTOR XM_CALLCONV XMVectorCos(
  [in] FXMVECTOR V
) noexcept;
```

同样有  
[XMVectorLog2](https://learn.microsoft.com/en-us/windows/win32/api/directxmath/nf-directxmath-xmvectorlog2)  
[XMVectorLogE](https://learn.microsoft.com/en-us/windows/win32/api/directxmath/nf-directxmath-xmvectorloge)  
[XMVectorPow](https://learn.microsoft.com/en-us/windows/win32/api/directxmath/nf-directxmath-xmvectorpow)  
[XMVectorSwizzle](https://learn.microsoft.com/en-us/windows/win32/api/directxmath/nf-directxmath-xmvectorswizzle)  
[XMVectorSaturate ](https://learn.microsoft.com/en-us/windows/win32/api/directxmath/nf-directxmath-xmvectorsaturate)  
[XMVectorMin](https://learn.microsoft.com/en-us/windows/win32/api/directxmath/nf-directxmath-xmvectormin)  
[XMVectorMax](https://learn.microsoft.com/en-us/windows/win32/api/directxmath/nf-directxmath-xmvectormax)

运算符重载也用了XM_CALLCONV和FXMVECTIR
```C++
ostream& XM_CALLCONV operator<<(ostream& os, FXMVECTOR v)
{
    XMFLOAT4 dest;
    XMStoreFloat4(&dest, v);

    os << "(" << dest.x << ", " << dest.y << ", " << dest.z << ", " << dest.w << ")";
    return os;
}
```

[XMVerifyCPUSupport](https://learn.microsoft.com/en-us/windows/win32/api/directxmath/nf-directxmath-xmverifycpusupport)  
Indicates if the DirectXMath Library supports the current platform.  
This is a run-time check of processor support and should be called at startup of the program before any DirectXMath functions or types are used.
