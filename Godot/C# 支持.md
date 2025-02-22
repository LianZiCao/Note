### 前置
下载 [.NET 版本](https://godotengine.org/download/) 的编辑器

可以在[这篇文章](https://docs.godotengine.org/zh-cn/4.x/tutorials/scripting/c_sharp/c_sharp_basics.html)中找怎么设置visual studio设置为编辑器，当然也要记得选择新创建的配置文件作为调试，要不然会启动失败
![photo](photo/屏幕截图%202025-02-23%20000038.png)

### 版本控制
C# 项目文件`.godot/mono`文件夹并不十分重要
`.godot` 文件夹下的所有内容都可以安全地添加到你的版本控制系统的忽略列表中

### C# API 
- 使用`PascalCase`命名法
- 附加 C# 脚本需要引用一个类，该类名需要匹配其文件名。
- 托管（类似传递函数指针）原生api需要原始`snake_case`的命名

### 常见陷阱
尝试改变一个 Node2D 的 X 坐标时：
```cs
public partial class MyNode2D : Node2D
{
    public override void _Ready()
    {
        Position.X = 100.0f;
        // CS1612: Cannot modify the return value of 'Node2D.Position' because
        // it is not a variable.
    }
}
```
C# 中的结构体（在这个例子中，是一个 Vector2 ）在赋值时会被复制，从一个属性或索引器中获取这样一个对象时，你得到的是它的一个副本，而不是它本身

解决方法
```cs
var newPosition = Position;
newPosition.X = 100.0f;
Position = newPosition;
```
参见ms的[参考文档](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/compiler-messages/cs1612)