#ASP.NET Core下将资源打包进dll
asp.net core下用`File Providers`访问抽象文件文件系统，如`IHostingEnvironment`中的webrootpath, contentpath, 就是就是两个 IFileProdier
```
    public interface IHostingEnvironment
    {
        string EnvironmentName { get; set; }
        string ApplicationName { get; set; }
        string WebRootPath { get; set; }.
        IFileProvider WebRootFileProvider { get; set; }
        string ContentRootPath { get; set; }
        IFileProvider ContentRootFileProvider { get; set; }
    }
```

#修改项目文件
```PropertyGroup```加入
<GenerateEmbeddedFilesManifest>true</GenerateEmbeddedFilesManifest>

 <ItemGroup>
    <EmbeddedResource Include="Images/*.*" />
</ItemGroup>  
支持通配符

#strtup，加入下列代码
//IFileProvider fileProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
IFileProvider fileProvider = new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
app.UseStaticFiles(new StaticFileOptions { FileProvider = fileProvider });

#如何查看程序集嵌入了哪些资源
```
public string[] FilesList()
{
     return Assembly.GetEntryAssembly().GetManifestResourceNames();
}
```
