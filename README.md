# NLog.Extended.Standard
[![Version](https://img.shields.io/nuget/vpre/NLog.Extended.Standard.svg)](https://www.nuget.org/packages/NLog.Extended.Standard)  
Implement the custom NLog target and layout renderers with .NET Standard 2.0.   

## Now Available Features ##   
### Targets ###
* **AzureAppendBlob** - Append log to Azure storage blobs   
### Layout Renderers(Layouts) ###
* **appsetting** - Access configuration in appsettings.json file       

## How To Use ##   
Install the [NLog.Extended.Standard](https://www.nuget.org/packages/NLog.Extended.Standard) package from NuGet. You need add NLog 4.5 or higher, then put the syntax in your NLog configuration below:

    <nlog>
        <extensions>
            <add assembly="NLog.Extended.Standard" />
        </extensions>
    </nlog>
