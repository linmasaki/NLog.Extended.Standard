# NLog.Extended.Standard
[![Version](https://img.shields.io/nuget/vpre/NLog.Extended.Standard.svg)](https://www.nuget.org/packages/NLog.Extended.Standard) 
[![Downloads](https://img.shields.io/nuget/dt/NLog.Extended.Standard.svg)](https://www.nuget.org/packages/NLog.Extended.Standard)  
Implement NLog various extension of Targets and Layout Renderers to use in .NET Core App(1.x ~ 2.x).  

## Now Available Features ##   
### Targets ###
* **AzureAppendBlob** - Append log to Azure storage blobs   
### Layout Renderers(Layouts) ###
* **appsettings** - Access configuration in appsettings.json (or appsettings.\<EnvironmentName\>.json) file       

## How To Use ##   
Install the [NLog.Extended.Standard](https://www.nuget.org/packages/NLog.Extended.Standard) package from NuGet. You need add NLog 4.5 or higher, then put the syntax in your NLog configuration below:

```xml
<nlog>
    <extensions>
        <add assembly="NLog.Extended.Standard" />
    </extensions>
</nlog>
```

### AzureAppendBlob target configuration ###
The type name of target is ``AzureAppendBlob``.

* **layout** - (layout) Content text to write. 
* **connectionString** - (layout) The connection string of the storage account. Consult the Azure Portal to retrieve this. 
* **container** - (layout) The name of the blob container where logs will be placed. It will be created automatically when it does not exist. 
* **blobName** - (layout) The name of the blob to write to. It will be created automatically when it does not exist(only once, unless you set **forceCheck** to ``true``). 
* **forceCheck** - (bool) Check if the target blob exists for each write. ***Optional***. 

#### Example: ####

```xml
<targets async="true">
    <target xsi:type="AzureAppendBlob" 
            name="Azure" 
            layout="${longdate} ${uppercase:${level}} - ${message}" 
            connectionString="YourConnectionString" 
            container="YourContainer" 
            blobName="logs/${shortdate}.log" 
            forceCheck= "false" />
</targets>
<rules>
    <logger name="*" minlevel="Trace" writeTo="Azure"/>
</rules>
```

You can see [NLog Wiki](https://github.com/NLog/NLog) for more information about configuring NLog.   
#### Note: ####   
If you only need ``AzureAppendBlob`` target, check [this](https://www.nuget.org/packages/NLog.AzureAppendBlob.Standard).  

### Appsettings layout renderer configuration ###
The name of layout renderer is ``appsettings``.  

#### Pay Attention ####
The previous version was ``appsetting``, you must modify it. (~~``appsetting``~~ -> ``appsettings``)

#### Configuration Syntax & Parameters ####
```xml
${appsettings:name=String.String2.String3:default=String}
```
* **name** - Key in your appsettings.\<EnvironmentName\>.json file. If it has a multi-level hierarchy that you want to access, you can separate by a dot. ***Required***.
* **default** - Default value if not present. ***Optional***.

#### Example: ####
Target appsettings.json

```json
    {
        "Mode":"Prod",
        "Options":{
            "StorageConnectionString":"abcdefg123456789",
            "Container":"YourProdContainer"
        }
    }
```

* **``${appsettings:name=Mode}``** - Get "Prod" in this case.
* **``${appsettings:name=Options.StorageConnectionString}``** - Get "abcdefg123456789" in this case.
* **``${appsettings:name=Options.StorageConnectionString2:default=DefaultString}``** - Get "DefaultString" in this case.

#### Set Explicit Configuration ####
In some cases, the library may not work correctly (e.g., always access incorrect appsettings.json). You can set the configuration directly by use the global property ``AppSettings`` before you start logging work as follows

```C#
using NLog.Appsettings.Standard;

..........

AppSettingsLayoutRenderer.AppSettings = new ConfigurationBuilder().SetBasePath(Directory.GetCurrentDirectory())
                                            .AddJsonFile("appsettings.json").AddJsonFile($"appsettings.Development.json", optional: true)
                                            .Build();
..........
```

#### Note: ####   
If you only need ``Appsettings`` layout renderer, check [this](https://www.nuget.org/packages/NLog.Appsettings.Standard).  

## Test App ##
NLog.Extended.Standard.Test is a console program that is preconfigured to use the ``AzureAppendBlob`` target and ``appsettings`` layout renderer. To test it, you'll have to create an Azure storage account and a blob account. Lastly, enjoy it!!!  

## Reference ## 
[NLog.AzureAppendBlob](https://github.com/heemskerkerik/NLog.AzureAppendBlob) by [Erik Heemskerk](https://github.com/heemskerkerik)   
[NLog.Extended](https://github.com/nlog/nlog/wiki/AppSetting-Layout-Renderer) by [NLog](http://nlog-project.org/)
