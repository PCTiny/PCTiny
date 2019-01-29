创建一个简单的Revit外部命令程序【Revit2017+VS2015】：
1、打开VS，我的版本是VS2015，点击【新建项目】→【Visual C#】→【类库】，然后输入程序名称，如：Hello World。

2、点击【项目】→【添加引用】→【浏览】，在Revit安装目录下找到【RevitAPI.dll】和【RevitAPIUI.dll】并添加。

3、在【解决方案资源管理器】中，右键【RevitAPI】和【RevitAPIUI】，点击【属性】，将属性【复制本地】改False。   
    （如果不修改此项属性，则会将大量引用文件复制到输出目录中）
    
4、【解决方案资源管理器】中，修改类名，默认为Class1。（如果不想修改类名，可以跳过此步骤）

5、类中代码如下：

```
using Autodesk.Revit;
using Autodesk.Revit.DB;
using Autodesk.Revit.UI;
using Autodesk.Revit.Attributes;
namespace Hello_World
{
    [Transaction(TransactionMode.Manual)]
    public class Command:IExternalCommand
    {
        public Result Execute(ExternalCommandData commandData, ref string message, ElementSet elements)
        {
            try
            {
                TaskDialog.Show("Hello", "First Revit Program.");
            }
            catch (Exception e)
            {
                message = e.Message;
                return Result.Failed;
            }
            return Result.Succeeded;
        }
    }
}  
```

6、选择【项目】→【属性】→【调试】→【启动外部程序】，找到Revit安装目录，选择Revit.exe，例如：我的是D:\Revit2017\Revit 2017\Revit.exe，点击保存。

7、完成以上步骤后，便可以【启动】项目了，项目编译完之后，类库文件(Hello World.dll)便输出到了你的Debug文件夹中。
那么在得到了程序拓展文件(XXX.dll)之后，我们该如何把它加入到Revit中呢？

8、找到文件夹【C:\Users\Administrator\AppData\Roaming\Autodesk\Revit\Addins\2017】（我使用的是Win10系统，Win7自行对应）
      新建一个记事本【Hello World.addin】文件，内容如下：
```
<?xml version="1.0" encoding="utf-8"?>
<RevitAddIns>
	<AddIn Type="Command">
	<VendorId>ACID</VendorId>
	<Text>Hello Revit</Text>
	<Description>Hello World to Revit.</Description>
	<FullClassName>Hello_World.Command</FullClassName>
	<Assembly>F:\MyTestDemos\Hello World\Hello World\bin\Debug\Hello World.dll</Assembly>
	<AddInId>E2CB270D-2889-48AD-8193-C93663BE7AD9</AddInId>
	</AddIn>
</RevitAddIns>
```

【VendorId】，开发商Id，可以自己随意取名。
【Text】，Revit中插件的名称。
【Description】，插件的描述信息。（可不写这项）
【FullClassName】，类名。注意：得填写完整的【命名空间.类名】。
【Assembly】，需要加载的程序集的完整路径。
【AddIn】，这一项在VS的【工具】→【创建GUID】中获得。

完成以上所有步骤后，打开Revit应用程序，【附加模块】→【外部工具】→，就可以看见你的Hello World插件了。
![这里写图片描述](https://img-blog.csdn.net/20160804171357673)
![这里写图片描述](https://img.blog.csdn.net/20160804171416841)
 







