[[_TOC_]]

# XamarinRiderDev
Guide on how to develop xamarin apps in rider under linux.
The System is a clean installed POP OS 20.10

Well this is how I do it. ( Run scripts as sudo )


# BuildTools
## Xamarin, Java, Dotnet, Mono
- Grab one of the latest BuildArtifacts from [Azure](https://dev.azure.com/xamarin/public/_build?definitionId=48&_a=summary)
( [maybeThatOne?](https://artprodcus3.artifacts.visualstudio.com/Ad0adf05a-e7d7-4b65-96fe-3f3884d42038/6fd3d886-57a5-4e31-8db7-52a1b47c07a8/_apis/artifact/cGlwZWxpbmVhcnRpZmFjdDovL3hhbWFyaW4vcHJvamVjdElkLzZmZDNkODg2LTU3YTUtNGUzMS04ZGI3LTUyYTFiNDdjMDdhOC9idWlsZElkLzI5NzYzL2FydGlmYWN0TmFtZS9JbnN0YWxsZXJzKy0rTGludXg1/content?format=zip) if it is still there) 


```bash
echo "------------------------------"
echo " Install .NET Core and Mono SDK: "
echo "------------------------------"
wget -q https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O dotnetRepo.deb
dpkg -i dotnetRepo.deb

apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
echo "deb https://download.mono-project.com/repo/ubuntu stable-focal main" | sudo tee /etc/apt/sources.list.d/mono-official-stable.list
apt update
apt install mono-complete dotnet-sdk-3.1 -y


echo "------------------------------"
echo " Install Xamarin "
echo "------------------------------"
wget "https://artprodcus3.artifacts.visualstudio.com/Ad0adf05a-e7d7-4b65-96fe-3f3884d42038/6fd3d886-57a5-4e31-8db7-52a1b47c07a8/_apis/artifact/cGlwZWxpbmVhcnRpZmFjdDovL3hhbWFyaW4vcHJvamVjdElkLzZmZDNkODg2LTU3YTUtNGUzMS04ZGI3LTUyYTFiNDdjMDdhOC9idWlsZElkLzI5NzYzL2FydGlmYWN0TmFtZS9JbnN0YWxsZXJzKy0rTGludXg1/content?format=zip" -O xamarin.zip

apt install openjdk-11-jdk lxd -y
apt install -f -y

unzip xamarin.zip
dpkg -i ./Installers\ -\ Linux/xamarin.android-oss_11.1.99.0_amd64.deb

```

## Android Studio
Afterwards install [Android Studio](https://developer.android.com/studio/), open it and go to *Configure (bottom) Settings -> System Settings -> Android*

### SDK Platforms:
Install your Target APIs
![platforms](pics/sdkPlats.png)

### SDK Tools
![sdkTools](pics/sdkTools.png)


## Rider

### Settings
Install [Rider](https://www.jetbrains.com/rider/download/#section=linux), open it and go to *Configure (bottom) Settings -> Build, Execution, Deployment -> Android*

*Use your AndroidHome and correct ndk version!*

![riderSettings](pics/xamarinRider.png)

## Maybe still some issues
### Strings.xml
Creating a new project creates ./Resources/values/Strings.xml.  
Rider won't recognize it unless its in lowercase.  

```bash
$ mv ./Resources/values/Strings.xml ./Resources/values/strings.xml
```

### SuppoirtedAbis
Append the *SupportedAbis* where needed.

```xml
<AndroidSupportedAbis>arm64-v8a;armeabi-v7a;x86;x86_64</AndroidSupportedAbis>
```

```xml
    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|AnyCPU' ">
        <DebugSymbols>True</DebugSymbols>
        <DebugType>portable</DebugType>
        <Optimize>False</Optimize>
        <OutputPath>bin\Debug\</OutputPath>
        <DefineConstants>DEBUG;TRACE</DefineConstants>
        <ErrorReport>prompt</ErrorReport>
        <WarningLevel>4</WarningLevel>
        <AndroidUseSharedRuntime>True</AndroidUseSharedRuntime>
        <AndroidSupportedAbis>arm64-v8a;armeabi-v7a;x86;x86_64</AndroidSupportedAbis>
        <AndroidLinkMode>None</AndroidLinkMode>
        <EmbedAssembliesIntoApk>False</EmbedAssembliesIntoApk>
    </PropertyGroup>
    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|AnyCPU' ">
        <DebugSymbols>True</DebugSymbols>
        <DebugType>pdbonly</DebugType>
        <Optimize>True</Optimize>
        <OutputPath>bin\Release\</OutputPath>
        <DefineConstants>TRACE</DefineConstants>
        <ErrorReport>prompt</ErrorReport>
        <WarningLevel>4</WarningLevel>
        <AndroidManagedSymbols>true</AndroidManagedSymbols>
        <AndroidUseSharedRuntime>False</AndroidUseSharedRuntime>
        <AndroidSupportedAbis>arm64-v8a;armeabi-v7a;x86;x86_64</AndroidSupportedAbis>
        <AndroidLinkMode>SdkOnly</AndroidLinkMode>
        <EmbedAssembliesIntoApk>True</EmbedAssembliesIntoApk>
    </PropertyGroup>
```

In My latest tests I didnt need them at all
