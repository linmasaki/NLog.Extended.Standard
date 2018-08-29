# NLog.Extended.Standard
[![Version](https://img.shields.io/nuget/vpre/NLog.Extended.Standard.svg)](https://www.nuget.org/packages/NLog.Extended.Standard)  
Implement NLog various extension of Targets(AzureAppendBlob) and LayoutRenderer(appsettings) with .NET Standard 2.0.   

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
The target's type name is ``AzureAppendBlob``.

* **connectionString** - (layout)The connection string for the storage account to use. To Retrieve this at Azure Portal.
* **container** - (layout)The name of the blob container where logs will be placed. Will be created when it does not exist.
* **blobName** - (layout)Name of the blob to write to. Will be created when it does not exist.
* **layout** - (layout)Content text to write.   

#### Example: ####

```xml
<targets async="true">
    <target xsi:type="AzureAppendBlob" 
            name="Azure" 
            layout="${longdate} ${uppercase:${level}} - ${message}" 
            connectionString="YourConnectionString" 
            container="YourContainer" 
            blobName="logs/${shortdate}.log" />
</targets>
<rules>
    <logger name="*" minlevel="Trace" writeTo="Azure"/>
</rules>
```

You can see [NLog Wiki](https://github.com/NLog/NLog) for more information about configuring NLog.   
#### Note: ####   
If you only need ``AzureAppendBlob`` target, check [this](https://www.nuget.org/packages/NLog.AzureAppendBlob.Standard).  

### Appsettings layout renderer configuration ###
The layout renderer's name is ``appsettings``.  

#### Pay Attention ####
The previous version was ``appsetting``, you must modify it. ~~``appsetting``~~ -> ``appsettings``)

#### Configuration Syntax & Parameters ####
```xml
${appsettings:name=String.String2.String3:default=String}
```
* **name** - Key in your appsettings.\<EnvironmentName\>.json file. If it has a multi-level hierarchy that you want to access, you can separate by a dot. Required.
* **default** - Default value if not present. Optional.

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

## Building ##
The project is a .NET Standard 2.0 project. If you wish to build it yourself, you'll need install Visual Studio 2017 or Visual Studio Code.

## Test App ##
NLog.Extended.Standard.Test is a console program that is preconfigured to use the ``AzureAppendBlob`` target and ``appsettings`` layout renderer. To test it, you'll have to create an Azure storage account and a blob account. Lastly, enjoy it!!!  

## Reference ## 
[NLog.AzureAppendBlob](https://github.com/heemskerkerik/NLog.AzureAppendBlob) by [Erik Heemskerk](https://github.com/heemskerkerik)   
[NLog.Extended](https://github.com/nlog/nlog/wiki/AppSetting-Layout-Renderer) by [NLog](http://nlog-project.org/)
