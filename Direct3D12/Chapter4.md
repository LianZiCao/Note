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

![图片](Chapter4/2.png)  

## 交换链和页面翻转
为了避免动画中出现画面闪烁的线下，最好将动画帧完整地绘制在一种称为后台缓冲区的离屏纹理内.  
两种纹理缓冲区：**前台缓冲区**（front buffer）和**后台缓冲区**（back buffer）  
前后台缓冲互换的操作称为**呈现**/提交/显示（presenting）只要交换指向但钱前台缓冲区和后台缓冲区的两个指针即可实现。
![图片](Chapter4/3.png)  

前台缓冲区和后台缓冲区构成了**交换链**（swap chain），在Direct3D中用IDXGISwapChain接口来表示。  
使用两个缓冲区（前台和后台）的情况称为双缓冲（double buffering）  
使用三个缓冲去就叫做三重缓冲（triple buffering）
后台缓冲区是一个纹理，构成纹理的基本元素又称**纹素**（texel），常俗称为像素，但是当谈及像素和纹素的映射关系时，必须将这两个概念区分。

关于[垂直同步](https://en.wikipedia.org/wiki/Screen_tearing#Vertical_synchronization)

## 深度缓冲
深度缓冲区（depth buffer）这种纹理资源存储的并非图像数据，而是特定像素的深度信息。深度值范围[0.0,1.0]，0.0表示观察者在视锥体（view frustum）中能看到离自己最近的物体，1.0则是能看到的最远的物体。  
深度缓冲中的元素和与后台缓存区内像素呈现一一对应的关系。  
为了确定不同物体间像素的前后顺序，Direct3D采用了深度缓冲/z缓冲的技术，使用了深度缓冲物体的绘制顺序也就无关紧要了。
按照场景中的物体由远及近绘制的缺点：对大量的数据按从后至前的绘制顺序进行排序并且还涉及集合体相交的问题  

在开始渲染之前，后台缓冲区会被清理为默认颜色，深度缓冲区也将被清除为默认值——通常为1.0（即像素能够抓到的最远深度值）  
![图片](Chapter4/4.png)  
一个应用程序不一定要用到模板缓冲区，但是一经使用，深度缓冲区将总是与模板缓冲区如随如影，共同进退。出于这个原因，深度缓冲区叫做深度缓冲区/模板缓冲区更为得体。

## 资源与描述符
![图片](Chapter4/5.png)  
![图片](Chapter4/6.png)  
指定资源数据，为GPU解释资源。无类型资源有灵活性，但是效率会偏低，建议当确实用到了这种基于不同试图对同一数据进行不同解释的灵活性才使用这种方式创建资源。  
视图（view）与描述符（descriptor）是同义词  
**描述符堆**（descriptor heap）中存有一系列描述符（可将其看作是描述符数组），本质上是存放用户程序中某种特定类型描述符的一块内存。我们需要为每一种类型的描述符都创建出单独的描述符堆。可以为同一种描述符类型创建出多个描述符堆。  
创建描述符的最佳时机为初始化期间。因为要进行检测和验证工作，所以最好不要在运行时（runtime）才创建描述符。

## 多重采样技术的原理
走样（aliasing）  
反走样/抗锯齿/反锯齿/反失真（antialiasing）  
超级采样（supersampling，简记为SSAA，即Super Sample Anti-Aliasing），使用四倍于屏幕分辨率大小的后台缓冲区和深度缓冲区，然后将以四个像素为一组进行解析（resolve，或称降采样，downsample）每组求平均值得到相对平滑的像素颜色。  
超级采样开销高昂，多重采样（multisampling，简记为MSAA，即MultiSample Anti-Aliasing）相对折中。并不需要对每一个子像素，而是计算一次像素中心位置处的颜色，基于可视性和覆盖性将得到的颜色信息分享给其子像素。  

## 利用Direct3D进行多重采样
填写[DXGI_SAMPLE_DESC](https://learn.microsoft.com/en-us/windows/win32/api/dxgicommon/ns-dxgicommon-dxgi_sample_desc)结构体
```C++
typedef struct DXGI_SAMPLE_DESC {
  UINT Count;
  UINT Quality;
} DXGI_SAMPLE_DESC;
```
Count成员制定了每个像素的采样次数，Quality成员则用于指示用户期望的图像质量级别（取决于纹理格式和每个像素的采样质量）  
根据给定的纹理格式和采样数量，可以用[ID3D12Device::CheckFeatureSupport method (d3d12.h)](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12device-checkfeaturesupport)查询对应的质量级别  
[D3D12_FEATURE_DATA_MULTISAMPLE_QUALITY_LEVELS structure (d3d12.h)](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_feature_data_multisample_quality_levels)  
（此事在后面功能支持的检测亦有记载）
![图片](Chapter4/9.png)  
![图片](Chapter4/10.png)  

## 功能级别
![图片](Chapter4/11.png)  
[Direct3D feature levels](https://learn.microsoft.com/en-us/windows/win32/direct3d11/overviews-direct3d-11-devices-downlevel-intro)  
“功能级别”为不同级别所支持的功能进行了严格的界定，如果用户的硬件不支持某特定功能级别用户程序理应回退至版本更低的功能级别，应该按照最新至最旧的顺序进行检测

## DirectX图形基础结构
DirectX图形基础结构（DirectX Graphics Infrastruture，DXGI，也作DirectX图形基础设施）是与Direct3D配合使用的API，设计其的基本理念是使多种图形API中所共有的底层任务能借助一组通用API来进行处理。例如交换链接口IDXGISwapChain实际上就属于DXGI API。DXGI还能处理一些其他的常用功能例如切换全屏模式，枚举显示适配器，还定义了Direct3D支持的各种表面格式信息（DXGI_FORMAT）  

IDXGIFactory是DXGI的关键接口之一，主要用于创建IDXGISwapChain接口以及枚举显示适配器。显示适配器（display adapter）是一种硬件设备（例如独立显卡）。适配器用接口IDXGIAdapter表示

[IDXGIFactory4 interface (dxgi1_4.h)](https://learn.microsoft.com/en-us/windows/win32/api/dxgi1_4/nn-dxgi1_4-idxgifactory4)  
注意其类方法[EnumAdapters](https://learn.microsoft.com/en-us/windows/win32/api/dxgi/nf-dxgi-idxgifactory-enumadapters)  
[IDXGIAdapter interface (dxgi.h)](https://learn.microsoft.com/en-us/windows/win32/api/dxgi/nn-dxgi-idxgiadapter)  

使用例
```C++
void D3DApp::LogAdapters()
{
    UINT i = 0;
    IDXGIAdapter* adapter = nullptr;
    std::vector<IDXGIAdapter*> adapterList;
    while(mdxgiFactory->EnumAdapters(i, &adapter) != DXGI_ERROR_NOT_FOUND)
    {
        DXGI_ADAPTER_DESC desc;
        adapter->GetDesc(&desc);

        std::wstring text = L"***Adapter: ";
        text += desc.Description;
        text += L"\n";

        OutputDebugString(text.c_str());

        adapterList.push_back(adapter);
        
        ++i;
    }

    for(size_t i = 0; i < adapterList.size(); ++i)
    {
        LogAdapterOutputs(adapterList[i]);
        ReleaseCom(adapterList[i]);
    }
}
```

每一台显示设备都是一个**显示输出**（display output，有时也作adapter output）实例，用IDXGIOutput接口来表示  ，例如枚举与某块适配器关联的所有显示输出
```C++
void D3DApp::LogAdapterOutputs(IDXGIAdapter* adapter)
{
    UINT i = 0;
    IDXGIOutput* output = nullptr;
    while(adapter->EnumOutputs(i, &output) != DXGI_ERROR_NOT_FOUND)
    {
        DXGI_OUTPUT_DESC desc;
        output->GetDesc(&desc);
        
        std::wstring text = L"***Output: ";
        text += desc.DeviceName;
        text += L"\n";
        OutputDebugString(text.c_str());

        LogOutputDisplayModes(output, mBackBufferFormat);

        ReleaseCom(output);

        ++i;
    }
}
```
在显卡驱动正常工作的情况下，"Microsoft Basic Render Driver"不会关联任何显示输出

每种设备所支持的显示模式可以用下列DXGI_MODE_DESC结构体的数据成员来表示  
[DXGI_MODE_DESC structure](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/legacy/bb173064(v=vs.85))  
[DXGI_MODE_SCANLINE_ORDER enumeration](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/legacy/bb173067(v=vs.85))  
[DXGI_MODE_SCALING enumeration](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/legacy/bb173066(v=vs.85))  
对显示输出所支持的全部显示模式的示例
```C++
void D3DApp::LogOutputDisplayModes(IDXGIOutput* output, DXGI_FORMAT format)
{
    UINT count = 0;
    UINT flags = 0;

    // Call with nullptr to get list count.
    output->GetDisplayModeList(format, flags, &count, nullptr);

    std::vector<DXGI_MODE_DESC> modeList(count);
    output->GetDisplayModeList(format, flags, &count, &modeList[0]);

    for(auto& x : modeList)
    {
        UINT n = x.RefreshRate.Numerator;
        UINT d = x.RefreshRate.Denominator;
        std::wstring text =
            L"Width = " + std::to_wstring(x.Width) + L" " +
            L"Height = " + std::to_wstring(x.Height) + L" " +
            L"Refresh = " + std::to_wstring(n) + L"/" + std::to_wstring(d) +
            L"\n";

        ::OutputDebugString(text.c_str());
    }
}
```

在进入全屏模式时，枚举显示模式就十分重要，为了获得最优的全屏性能，我们所指定的显示模式（包括刷新率）一定要与显示器支持的显示模式完全匹配  

## 功能支持的检测
[ID3D12Device::CheckFeatureSupport method (d3d12.h)](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12device-checkfeaturesupport)  
[D3D12_FEATURE enumeration (d3d12.h)](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_feature)  
方法的原型为
```c++
HRESULT CheckFeatureSupport(
            D3D12_FEATURE Feature,
  [in, out] void          *pFeatureSupportData,
            UINT          FeatureSupportDataSize
);
```
![图片](Chapter4/12.png)
![图片](Chapter4/13.png)
![图片](Chapter4/14.png)

## 资源驻留
Direct3D12中，应用程序通过控制资源在显存中的去留，主动管理资源的驻留的情况。D3D11则是系统自动管理。程序应当在短时间内由于显存中交换进出相同的资源，会引起过高的开销。最理想的情况是所清出的资源在短时间不会再次使用，例如游戏关卡或游戏场景的切换  
参见[Residency](https://learn.microsoft.com/en-us/windows/win32/direct3d12/residency)