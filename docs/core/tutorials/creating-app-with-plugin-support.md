---
title: 플러그 인을 사용하여 .NET Core 애플리케이션 만들기
description: 플러그 인을 지원하는 .NET Core 애플리케이션을 만드는 방법을 알아봅니다.
author: jkoritzinsky
ms.author: jekoritz
ms.date: 01/28/2019
ms.openlocfilehash: f2997c778b87ecd88c0fd2fadf491763066a4950
ms.sourcegitcommit: facefcacd7ae2e5645e463bc841df213c505ffd4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55739600"
---
# <a name="create-a-net-core-application-with-plugins"></a><span data-ttu-id="59a99-103">플러그 인을 사용하여 .NET Core 애플리케이션 만들기</span><span class="sxs-lookup"><span data-stu-id="59a99-103">Create a .NET Core application with plugins</span></span>

<span data-ttu-id="59a99-104">이 자습서에서는 다음을 수행하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-104">This tutorial shows you how to:</span></span>

- <span data-ttu-id="59a99-105">플러그 인을 지원하는 프로젝트를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-105">Structure a project to support plugins.</span></span>
- <span data-ttu-id="59a99-106">각 플러그 인을 로드하는 사용자 지정 <xref:System.Runtime.Loader.AssemblyLoadContext>를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-106">Create a custom <xref:System.Runtime.Loader.AssemblyLoadContext> to load each plugin.</span></span>
- <span data-ttu-id="59a99-107">`System.Runtime.Loader.AssemblyDependencyResolver` 형식을 사용하여 플러그 인에 종속성을 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-107">Use the `System.Runtime.Loader.AssemblyDependencyResolver` type to allow plugins to have dependencies.</span></span>
- <span data-ttu-id="59a99-108">빌드 아티팩트를 복사하기만 하면 쉽게 배포할 수 있는 작성자 플러그 인입니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-108">Author plugins that can be easily deployed by just copying the build artifacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="59a99-109">전제 조건</span><span class="sxs-lookup"><span data-stu-id="59a99-109">Prerequisites</span></span>

- <span data-ttu-id="59a99-110">[.NET Core 3.0 Preview 2 SDK](https://www.microsoft.com/net/core) 또는 최신 버전을 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-110">Install the [.NET Core 3.0 Preview 2 SDK](https://www.microsoft.com/net/core) or a newer version.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="59a99-111">애플리케이션 만들기</span><span class="sxs-lookup"><span data-stu-id="59a99-111">Create the application</span></span>

<span data-ttu-id="59a99-112">첫 번째 단계에서는 애플리케이션을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-112">The first step is to create the application:</span></span>

1. <span data-ttu-id="59a99-113">새 폴더를 만들고 해당 폴더에서 `dotnet new console -o AppWithPlugin`을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-113">Create a new folder, and in that folder run `dotnet new console -o AppWithPlugin`.</span></span> 
2. <span data-ttu-id="59a99-114">프로젝트를 쉽게 빌드하도록 하려면 Visual Studio 솔루션 파일을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-114">To make building the project easier, create a Visual Studio solution file.</span></span> <span data-ttu-id="59a99-115">동일한 폴더에서 `dotnet new sln`을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-115">Run `dotnet new sln` in the same folder.</span></span> 
3. <span data-ttu-id="59a99-116">`dotnet sln add AppWithPlugin/AppWithPlugin.csproj`를 실행하여 솔루션에 앱 프로젝트를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-116">Run `dotnet sln add AppWithPlugin/AppWithPlugin.csproj` to add the app project to the solution.</span></span>

<span data-ttu-id="59a99-117">이제 애플리케이션의 구조를 채우면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-117">Now we can fill in the skeleton of our application.</span></span> <span data-ttu-id="59a99-118">*AppWithPlugin/Program.cs* 파일의 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-118">Replace the code in the *AppWithPlugin/Program.cs* file with the following code:</span></span>

```csharp
using PluginBase;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Reflection;

namespace AppWithPlugin
{
    class Program
    {
        static void Main(string[] args)
        {
            try
            {
                if (args.Length == 1 && args[0] == "/d")
                {
                    Console.WriteLine("Waiting for any key...");
                    Console.ReadLine();
                }

                // Load commands from plugins.

                if (args.Length == 0)
                {
                    Console.WriteLine("Commands: ");
                    // Output the loaded commands.
                }
                else
                {
                    foreach (string commandName in args)
                    {
                        Console.WriteLine($"-- {commandName} --");

                        // Execute the command with the name passed as an argument.

                        Console.WriteLine();
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex);
            }
        }
    }
}

```

## <a name="create-the-plugin-interfaces"></a><span data-ttu-id="59a99-119">플러그 인 인터페이스 만들기</span><span class="sxs-lookup"><span data-stu-id="59a99-119">Create the plugin interfaces</span></span>

<span data-ttu-id="59a99-120">플러그 인을 사용하여 앱을 빌드하는 다음 단계에서는 플러그 인이 구현해야 하는 인터페이스를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-120">The next step in building an app with plugins is defining the interface the plugins need to implement.</span></span> <span data-ttu-id="59a99-121">앱과 플러그 인 간의 통신에 사용할 모든 형식을 포함하는 클래스 라이브러리를 만드는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-121">We suggest that you make a class library that contains any types that you plan to use for communicating between your app and plugins.</span></span> <span data-ttu-id="59a99-122">이 단계를 통해 전체 애플리케이션을 배포할 필요 없이 플러그 인 인터페이스를 패키지로 게시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-122">This division allows you to publish your plugin interface as a package without having to ship your full application.</span></span>

<span data-ttu-id="59a99-123">프로젝트의 루트 폴더에서 `dotnet new classlib -o PluginBase`를 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-123">In the root folder of the project, run `dotnet new classlib -o PluginBase`.</span></span> <span data-ttu-id="59a99-124">또한 `dotnet sln add PluginBase/PluginBase.csproj`를 실행하여 솔루션 파일에 프로젝트를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-124">Also, run `dotnet sln add PluginBase/PluginBase.csproj` to add the project to the solution file.</span></span> <span data-ttu-id="59a99-125">`PluginBase/Class1.cs` 파일을 삭제하고, `PluginBase` 폴더에서 다음 인터페이스 정의를 포함한 `ICommand.cs`라는 새 파일을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-125">Delete the `PluginBase/Class1.cs` file, and create a new file in the `PluginBase` folder named `ICommand.cs` with the following interface definition:</span></span>

[!code-csharp[the-plugin-interface](~/samples/core/extensions/AppWithPlugin/PluginBase/ICommand.cs)]

<span data-ttu-id="59a99-126">이 `ICommand` 인터페이스는 모든 플러그 인이 구현할 인터페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-126">This `ICommand` interface is the interface that all of the plugins will implement.</span></span>

<span data-ttu-id="59a99-127">이제 `ICommand` 인터페이스가 정의되어, 애플리케이션 프로젝트를 좀 더 채울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-127">Now that the `ICommand` interface is defined, the application project can be filled in a little more.</span></span> <span data-ttu-id="59a99-128">루트 폴더에서 `dotnet add AppWithPlugin\AppWithPlugin.csproj reference PluginBase\PluginBase.csproj` 명령을 사용하여 `AppWithPlugin` 프로젝트부터 `PluginBase` 프로젝트까지 참조를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-128">Add a reference from the `AppWithPlugin` project to the `PluginBase` project with the `dotnet add AppWithPlugin\AppWithPlugin.csproj reference PluginBase\PluginBase.csproj`  command from the root folder.</span></span>

<span data-ttu-id="59a99-129">지정된 파일 경로에서 플러그 인을 로드할 수 있도록 `// Load commands from plugins` 주석을 다음 코드 조각으로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-129">Replace the `// Load commands from plugins` comment with the following code snippet to enable it to load plugins from given file paths:</span></span>

```csharp
string[] pluginPaths = new string[]
{
    // Paths to plugins to load.
};

IEnumerable<ICommand> commands = pluginPaths.SelectMany(pluginPath =>
{
    Assembly pluginAssembly = LoadPlugin(pluginPath);
    return CreateCommands(pluginAssembly);
}).ToList();
```



<span data-ttu-id="59a99-130">그런 다음, `// Output the loaded commands` 주석을 다음 코드 조각으로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-130">Then replace the `// Output the loaded commands` comment with the following code snippet:</span></span>

```csharp
foreach (ICommand command in commands)
{
    Console.WriteLine($"{command.Name}\t - {command.Description}");
}
```

<span data-ttu-id="59a99-131">`// Execute the command with the name passed as an argument` 주석을 다음 코드 조각으로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-131">Replace the `// Execute the command with the name passed as an argument` comment with the following snippet:</span></span>

```csharp
ICommand command = commands.FirstOrDefault(c => c.Name == commandName);
if (command == null)
{
    Console.WriteLine("No such command is known.");
    return;
}

command.Execute();
```

<span data-ttu-id="59a99-132">마지막으로, 여기에 표시된 것처럼 `LoadPlugin` 및 `CreateCommands`라는 `Program` 클래스에 정적 메서드를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-132">And finally, add static methods to the `Program` class named `LoadPlugin` and `CreateCommands`, as shown here:</span></span>

```csharp
static Assembly LoadPlugin(string relativePath)
{
    throw new NotImplementedException();
}

static IEnumerable<ICommand> CreateCommands(Assembly assembly)
{
    int count = 0;

    foreach (Type type in assembly.GetTypes())
    {
        if (typeof(ICommand).IsAssignableFrom(type))
        {
            ICommand result = Activator.CreateInstance(type) as ICommand;
            if (result != null)
            {
                count++;
                yield return result;
            }
        }
    }

    if (count == 0)
    {
        string availableTypes = string.Join(",", assembly.GetTypes().Select(t => t.FullName));
        throw new ApplicationException(
            $"Can't find any type which implements ICommand in {assembly} from {assembly.Location}.\n" +
            $"Available types: {availableTypes}");
    }
}
```

## <a name="load-plugins"></a><span data-ttu-id="59a99-133">플러그 인 로드</span><span class="sxs-lookup"><span data-stu-id="59a99-133">Load plugins</span></span>

<span data-ttu-id="59a99-134">이제 애플리케이션은 로드된 플러그 인 어셈블리에서 명령을 올바르게 로드하고 인스턴스화할 수 있지만 플러그 인 어셈블리를 로드할 수는 없습니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-134">Now the application can correctly load and instantiate commands from loaded plugin assemblies, but it is still unable to load the plugin assemblies.</span></span> <span data-ttu-id="59a99-135">다음 콘텐츠를 사용하여 *AppWithPlugin* 폴더에 *PluginLoadContext.cs*라는 파일을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-135">Create a file named *PluginLoadContext.cs* in the *AppWithPlugin* folder with the following contents:</span></span>

[!code-csharp[loading-plugins](~/samples/core/extensions/AppWithPlugin/AppWithPlugin/PluginLoadContext.cs)]

<span data-ttu-id="59a99-136">`PluginLoadContext` 형식은 <xref:System.Runtime.Loader.AssemblyLoadContext> 형식에서 파생됩니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-136">The `PluginLoadContext` type derives from <xref:System.Runtime.Loader.AssemblyLoadContext>.</span></span> <span data-ttu-id="59a99-137">`AssemblyLoadContext` 형식은 어셈블리 버전이 충돌하지 않도록 하기 위해 개발자가 로드된 어셈블리를 다른 그룹으로 격리할 수 있는 런타임의 특수 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-137">The `AssemblyLoadContext` type is a special type in the runtime that allows developers to isolate loaded assemblies into different groups to ensure that assembly versions do not conflict.</span></span> <span data-ttu-id="59a99-138">또한 사용자 지정 `AssemblyLoadContext`는 어셈블리를 로드하고 기본 동작을 재정의하는 다른 경로를 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-138">Additionally, a custom `AssemblyLoadContext` can choose different paths to load assemblies from and override the default behavior.</span></span> <span data-ttu-id="59a99-139">`PluginLoadContext`는 .NET Core 3.0에 포함된 `AssemblyDependencyResolver` 형식의 인스턴스를 사용하여 경로에 대한 어셈블리 이름을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-139">The `PluginLoadContext` uses an instance of the `AssemblyDependencyResolver` type introduced in .NET Core 3.0 to resolve assembly names to paths.</span></span> <span data-ttu-id="59a99-140">`AssemblyDependencyResolver` 개체는 .NET 클래스 라이브러리에 대한 경로를 사용하여 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-140">The `AssemblyDependencyResolver` object is constructed with the path to a .NET class library.</span></span> <span data-ttu-id="59a99-141">경로가 `AssemblyDependencyResolver` 생성자에 전달된 클래스 라이브러리의 경우 *deps.json* 파일에 따라 상대 경로에 대한 어셈블리 및 네이티브 라이브러리를 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-141">It resolves assemblies and native libraries to their relative paths based on the *deps.json* file for the class library whose path was passed to the `AssemblyDependencyResolver` constructor.</span></span> <span data-ttu-id="59a99-142">사용자 지정 `AssemblyLoadContext`를 사용하면 플러그 인에 고유한 종속성을 포함할 수 있고, `AssemblyDependencyResolver`를 통해 정확한 종속성을 쉽게 로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-142">The custom `AssemblyLoadContext` enables plugins to have their own dependencies, and the `AssemblyDependencyResolver` makes it easy to correctly load the dependencies.</span></span>

<span data-ttu-id="59a99-143">이제 `AppWithPlugin` 프로젝트에는 `PluginLoadContext` 형식이 포함되므로 다음 본문을 사용하여 `Program.LoadPlugin` 메서드를 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-143">Now that the `AppWithPlugin` project has the `PluginLoadContext` type, update the `Program.LoadPlugin` method with the following body:</span></span>

```csharp
static Assembly LoadPlugin(string relativePath)
{
    // Navigate up to the solution root
    string root = Path.GetFullPath(Path.Combine(
        Path.GetDirectoryName(
            Path.GetDirectoryName(
                Path.GetDirectoryName(
                    Path.GetDirectoryName(
                        Path.GetDirectoryName(typeof(Program).Assembly.Location)))))));

    string pluginLocation = Path.GetFullPath(Path.Combine(root, relativePath.Replace('\\', Path.DirectorySeparatorChar)));
    Console.WriteLine($"Loading commands from: {pluginLocation}");
    PluginLoadContext loadContext = new PluginLoadContext(pluginLocation);
    return loadContext.LoadFromAssemblyName(new AssemblyName(Path.GetFileNameWithoutExtension(pluginLocation)));
}
```

<span data-ttu-id="59a99-144">플러그 인은 각 플러그 인에서 다른 `PluginLoadContext` 인스턴스를 사용하여 문제가 발생하지 않고 다르거나 충돌하는 종속성을 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-144">By using a different `PluginLoadContext` instance for each plugin, the plugins can have different or even conflicting dependencies without issue.</span></span>

## <a name="create-a-simple-plugin-with-no-dependencies"></a><span data-ttu-id="59a99-145">종속성이 없는 간단한 플러그 인 만들기</span><span class="sxs-lookup"><span data-stu-id="59a99-145">Create a simple plugin with no dependencies</span></span>

<span data-ttu-id="59a99-146">루트 폴더에서 다시 다음을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-146">Back in the root folder, do the following:</span></span>

1. <span data-ttu-id="59a99-147">`dotnet new classlib -o HelloPlugin`을 실행하여 `HelloPlugin`이라는 새 클래스 라이브러리 프로젝트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-147">Run `dotnet new classlib -o HelloPlugin` to create a new class library project named `HelloPlugin`.</span></span>
2. <span data-ttu-id="59a99-148">`dotnet sln add HelloPlugin/HelloPlugin.csproj`를 실행하여 `AppWithPlugin` 솔루션에 프로젝트를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-148">Run `dotnet sln add HelloPlugin/HelloPlugin.csproj` to add the project to the `AppWithPlugin` solution.</span></span> 
3. <span data-ttu-id="59a99-149">*HelloPlugin/Class1.cs* 파일을 다음 콘텐츠를 포함하는 *HelloCommand.cs*라는 파일로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-149">Replace the *HelloPlugin/Class1.cs* file with a file named *HelloCommand.cs* with the following contents:</span></span>

[!code-csharp[the-hello-plugin](~/samples/core/extensions/AppWithPlugin/HelloPlugin/HelloCommand.cs)]

<span data-ttu-id="59a99-150">이제 *HelloPlugin.csproj* 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-150">Now, open the *HelloPlugin.csproj* file.</span></span> <span data-ttu-id="59a99-151">결과는 다음과 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-151">It should look similar to the following:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netcoreapp3.0</TargetFramework>
  </PropertyGroup>

</Project>

```

<span data-ttu-id="59a99-152">다음 요소를 `<Project>` 태그 사이에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-152">In between the `<Project>` tags, add the following elements:</span></span>

```xml
<ItemGroup>
<ProjectReference Include="..\PluginBase\PluginBase.csproj">
    <Private>false</Private>
</ProjectReference>
</ItemGroup>
```

<span data-ttu-id="59a99-153">`<Private>false</Private>` 요소는 매우 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-153">The `<Private>false</Private>` element is very important.</span></span> <span data-ttu-id="59a99-154">그러면 MSBuild에서 *PluginBase.dll*을 HelloPlugin의 출력 디렉터리에 복사하지 않도록 지시합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-154">This tells MSBuild to not copy *PluginBase.dll* to the output directory for HelloPlugin.</span></span> <span data-ttu-id="59a99-155">*PluginBase.dll* 어셈블리가 출력 디렉터리에 표시되는 경우 `PluginLoadContext`는 *HelloPlugin.dll* 어셈블리를 로드할 때 거기서 해당 어셈블리를 찾아 함께 로드합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-155">If the *PluginBase.dll* assembly is present in the output directory, `PluginLoadContext` will find the assembly there and load it when it loads the *HelloPlugin.dll* assembly.</span></span> <span data-ttu-id="59a99-156">이 시점에서 `HelloPlugin.HelloCommand` 형식은 기본 로드 컨텍스트에 로드된 `ICommand` 인터페이스가 아닌 `HelloPlugin` 프로젝트의 출력 디렉터리에서 *PluginBase.dll*의 `ICommand` 인터페이스를 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-156">At this point, the `HelloPlugin.HelloCommand` type will implement the `ICommand` interface from the *PluginBase.dll* in the output directory of the `HelloPlugin` project, not the `ICommand` interface that is loaded into the default load context.</span></span> <span data-ttu-id="59a99-157">런타임에서 이러한 두 가지 형식을 다른 어셈블리와 다르게 인식하므로 `AppWithPlugin.Program.CreateCommands` 메서드는 해당 명령을 찾지 못합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-157">Since the runtime sees these two types as different types from different assemblies, the `AppWithPlugin.Program.CreateCommands` method will not find the commands.</span></span> <span data-ttu-id="59a99-158">결과적으로, 플러그 인 인터페이스를 포함하는 어셈블리에 대한 참조에 `<Private>false</Private>` 메타데이터가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-158">As a result, the `<Private>false</Private>` metadata is required for the reference to the assembly containing the plugin interfaces.</span></span>

<span data-ttu-id="59a99-159">이제 `HelloPlugin` 프로젝트가 완료되었으므로 `HelloPlugin` 플러그 인을 찾을 수 있는 위치를 인식하도록 `AppWithPlugin` 프로젝트를 업데이트해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-159">Now that the `HelloPlugin` project is complete, we should update the `AppWithPlugin` project to know where the `HelloPlugin` plugin can be found.</span></span> <span data-ttu-id="59a99-160">`// Paths to plugins to load` 주석 뒤에 `@"HelloPlugin\bin\Debug\netcoreapp3.0\HelloPlugin.dll"`을 `pluginPaths` 배열의 요소로 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-160">After the `// Paths to plugins to load` comment, add `@"HelloPlugin\bin\Debug\netcoreapp3.0\HelloPlugin.dll"` as an element of the `pluginPaths` array.</span></span>

## <a name="create-a-plugin-with-library-dependencies"></a><span data-ttu-id="59a99-161">라이브러리 종속성을 포함한 플러그 인 만들기</span><span class="sxs-lookup"><span data-stu-id="59a99-161">Create a plugin with library dependencies</span></span>

<span data-ttu-id="59a99-162">모든 플러그 인은 "Hello World"보다 복잡하며, 여러 플러그 인에는 다른 라이브러리에 대한 종속성이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-162">Almost all plugins are more complex than a simple "Hello World", and many plugins have dependencies on other libraries.</span></span> <span data-ttu-id="59a99-163">샘플에서 `JsonPlugin` 및 `OldJson` 플러그 인 프로젝트는 `Newtonsoft.Json`에 대한 NuGet 패키지 종속성을 포함한 플러그 인의 두 가지 예제를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-163">The `JsonPlugin` and `OldJson` plugin projects in the sample show two examples of plugins with NuGet package dependencies on `Newtonsoft.Json`.</span></span> <span data-ttu-id="59a99-164">프로젝트 파일 자체에는 프로젝트 참조에 대한 특수 정보가 없으며, (`pluginPaths` 배열에 플러그 인 경로를 추가하면) AppWithPlugin 앱과 동일하게 실행하는 경우에도 플러그 인은 완벽하게 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-164">The project files themselves do not have any special information for the project references, and (after adding the plugin paths to the `pluginPaths` array) the plugins run perfectly, even if run in the same execution of the AppWithPlugin app.</span></span> <span data-ttu-id="59a99-165">그러나 이러한 프로젝트는 참조된 어셈블리를 출력 디렉터리에 복사하지 않습니다. 따라서 어셈블리는 플러그 인이 작동하도록 사용자의 머신에 표시되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-165">However, these projects do not copy the referenced assemblies to their output directory, so the assemblies need to be present on the user's machine for the plugins to work.</span></span> <span data-ttu-id="59a99-166">이 문제는 두 가지 방법으로 해결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-166">There are two ways to work around this problem.</span></span> <span data-ttu-id="59a99-167">첫 번째 방법은 클래스 라이브러리를 게시하는 `dotnet publish` 명령을 사용하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-167">The first option is to use the `dotnet publish` command to publish the class library.</span></span> <span data-ttu-id="59a99-168">또는 플러그 인에서 `dotnet build`의 출력을 사용하려는 경우 플러그 인의 프로젝트 파일에서 `<PropertyGroup>` 태그 사이에 `<CopyLocalLockFileAssemblies>true</CopyLocalLockFileAssemblies>` 속성을 추가하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-168">Alternatively, if you want to be able to use the output of `dotnet build` for your plugin, you can add the `<CopyLocalLockFileAssemblies>true</CopyLocalLockFileAssemblies>` property between the `<PropertyGroup>` tags in the plugin's project file.</span></span> <span data-ttu-id="59a99-169">예제는 `XcopyablePlugin` 플러그 인 프로젝트를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="59a99-169">See the `XcopyablePlugin` plugin project for an example.</span></span>

## <a name="other-plugin-examples-in-the-sample"></a><span data-ttu-id="59a99-170">이 샘플의 다른 플러그 인 예제</span><span class="sxs-lookup"><span data-stu-id="59a99-170">Other plugin examples in the sample</span></span>

<span data-ttu-id="59a99-171">`AssemblyDependencyResolver` 개체는 지역화된 위성 어셈블리뿐만 아니라 NuGet 패키지에 포함된 네이티브 라이브러리를 확인할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-171">The `AssemblyDependencyResolver` object can also resolve native libraries included in NuGet packages as well as localized satellite assemblies.</span></span> <span data-ttu-id="59a99-172">`UVPlugin` 및 `FrenchPlugin`은 각각 이러한 시나리오를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-172">The `UVPlugin` and `FrenchPlugin` demonstrate these scenarios respectively.</span></span>

## <a name="how-to-reference-a-plugin-interface-assembly-defined-in-a-nuget-package"></a><span data-ttu-id="59a99-173">NuGet 패키지에 정의된 플러그 인 인터페이스 어셈블리를 참조하는 방법</span><span class="sxs-lookup"><span data-stu-id="59a99-173">How to reference a plugin interface assembly defined in a NuGet package</span></span>

<span data-ttu-id="59a99-174">`A.PluginBase`라는 NuGet 패키지에 정의된 플러그 인 인터페이스가 포함된 앱 A가 있다고 가정하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-174">Let's say that there is an app A that has a plugin interface defined in the NuGet package named `A.PluginBase`.</span></span> <span data-ttu-id="59a99-175">플러그 인 프로젝트에서 패키지를 올바르게 참조하려면 어떻게 할까요?</span><span class="sxs-lookup"><span data-stu-id="59a99-175">How do you reference the package correctly in your plugin project?</span></span> <span data-ttu-id="59a99-176">프로젝트 참조의 경우 프로젝트 파일의 `ProjectReference` 요소에서 `<Private>false</Private>` 메타데이터를 사용하면 dll이 출력에 복사되지 않도록 방지합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-176">For project references, using the `<Private>false</Private>` metadata on the `ProjectReference` element in the project file prevented the dll from being copied to the output.</span></span>

<span data-ttu-id="59a99-177">`A.PluginBase` 패키지를 올바르게 참조하기 위해 프로젝트 파일에서 `<PackageReference>` 요소를 다음으로 변경하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-177">To correctly reference the `A.PluginBase` package, you want to change the `<PackageReference>` element in the project file to the following:</span></span>

```xml
<PackageReference Include="A.PluginBase" Version="1.0.0">
    <ExcludeAssets>runtime</ExcludeAssets>
</PackageReference>
```

<span data-ttu-id="59a99-178">그러면 `A.PluginBase` 어셈블리가 플러그 인의 출력 디렉터리에 복사되지 않도록 방지하고, 플러그 인에서 A의 `A.PluginBase` 버전을 사용하는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-178">This prevents the `A.PluginBase` assemblies from being copied to the output directory of your plugin and ensures that your plugin will use A's version of `A.PluginBase`.</span></span>

## <a name="plugin-target-framework-recommendations"></a><span data-ttu-id="59a99-179">플러그 인 대상 프레임워크 권장 사항</span><span class="sxs-lookup"><span data-stu-id="59a99-179">Plugin target framework recommendations</span></span>

<span data-ttu-id="59a99-180">로드 중인 플러그 인 종속성이 *deps.json* 파일을 사용하기 때문에 플러그 인의 대상 프레임워크와 관련된 알려진 과제가 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-180">Because plugin dependency loading uses the *deps.json* file, there is a gotcha related to the plugin's target framework.</span></span> <span data-ttu-id="59a99-181">특히, 사용자의 플러그 인은 .NET Standard 버전이 아닌 .NET Core 3.0과 같은 런타임을 대상으로 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-181">Specifically, your plugins should target a runtime, such as .NET Core 3.0, instead of a version of .NET Standard.</span></span> <span data-ttu-id="59a99-182">`deps.json` 파일은 해당 프로젝트가 대상으로 하는 프레임워크를 기반으로 생성됩니다. 다수의 .NET Standard 호환 패키지가 .NET Standard에 빌드하는 참조 어셈블리 및 특정 런타임에 대한 구현 어셈블리를 제공하므로 `deps.json`은 구현 어셈블리를 잘못 확인하거나 원하는 .NET Core 버전이 아닌 어셈블리의 .NET Standard 버전을 대상으로 할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59a99-182">The `deps.json` file is generated based on which framework the project targets, and since many .NET Standard-compatible packages ship reference assemblies for building against .NET Standard and implementation assemblies for specific runtimes, the `deps.json` may not correctly see implementation assemblies, or it may grab the .NET Standard version of an assembly instead of the .NET Core version you expect.</span></span>