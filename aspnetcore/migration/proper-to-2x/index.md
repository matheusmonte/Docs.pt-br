---
title: Migrando do ASP.NET para o ASP.NET Core 2.0
author: isaac2004
description: "Este documento de referência fornece diretrizes para migrar aplicativos de API Web ou ASP.NET MVC existentes para o ASP.NET Core 2.0."
keywords: ASP.NET Core, MVC, migrando
ms.author: scaddie
manager: wpickett
ms.date: 08/27/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/proper-to-2x/index
ms.openlocfilehash: 6e32ee64970bac5d7f09207e570f543477745b60
ms.sourcegitcommit: 8cafdd1dd409d5070d227100ba0e094c779ac47b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2017
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a><span data-ttu-id="5b85c-104">Migrando do ASP.NET para o ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="5b85c-104">Migrating from ASP.NET to ASP.NET Core 2.0</span></span>

<span data-ttu-id="5b85c-105">Por [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="5b85c-105">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="5b85c-106">Este artigo serve como um guia de referência para migração de aplicativos ASP.NET para o ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="5b85c-106">This article serves as a reference guide for migrating ASP.NET applications to ASP.NET Core 2.0.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5b85c-107">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="5b85c-107">Prerequisites</span></span>

* <span data-ttu-id="5b85c-108">[SDK do .NET Core 2.0.0](https://dot.net/core) ou posterior.</span><span class="sxs-lookup"><span data-stu-id="5b85c-108">[.NET Core 2.0.0 SDK](https://dot.net/core) or later.</span></span>
* <span data-ttu-id="5b85c-109">[Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) versão 15.3 ou posterior com a carga de trabalho do **ASP.NET e desenvolvimento Web**.</span><span class="sxs-lookup"><span data-stu-id="5b85c-109">[Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) version 15.3 or later with the **ASP.NET and web development** workload.</span></span>

## <a name="target-frameworks"></a><span data-ttu-id="5b85c-110">Frameworks de destino</span><span class="sxs-lookup"><span data-stu-id="5b85c-110">Target Frameworks</span></span>
<span data-ttu-id="5b85c-111">Projetos do ASP.NET Core 2.0 oferecem aos desenvolvedores a flexibilidade de direcionamento para o .NET Core, .NET Framework ou ambos.</span><span class="sxs-lookup"><span data-stu-id="5b85c-111">ASP.NET Core 2.0 projects offer developers the flexibility of targeting .NET Core, .NET Framework, or both.</span></span> <span data-ttu-id="5b85c-112">Consulte [Escolhendo entre o .NET Core e .NET Framework para aplicativos de servidor](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) para determinar qual estrutura de destino é mais apropriada.</span><span class="sxs-lookup"><span data-stu-id="5b85c-112">See [Choosing between .NET Core and .NET Framework for server apps](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) to determine which target framework is most appropriate.</span></span>

<span data-ttu-id="5b85c-113">Ao usar o .NET Framework como destino, projetos precisam fazer referência a pacotes NuGet individuais.</span><span class="sxs-lookup"><span data-stu-id="5b85c-113">When targeting .NET Framework, projects need to reference individual NuGet packages.</span></span>

<span data-ttu-id="5b85c-114">Usar o .NET Core como destino permite que você elimine várias referências de pacote explícitas, graças ao [metapacote](xref:fundamentals/metapackage) do ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="5b85c-114">Targeting .NET Core allows you to eliminate numerous explicit package references, thanks to the ASP.NET Core 2.0 [metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="5b85c-115">Instale o metapacote `Microsoft.AspNetCore.All` em seu projeto:</span><span class="sxs-lookup"><span data-stu-id="5b85c-115">Install the `Microsoft.AspNetCore.All` metapackage in your project:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="5b85c-116">Quando o metapacote é usado, nenhum pacote referenciado no metapacote é implantado com o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5b85c-116">When the metapackage is used, no packages referenced in the metapackage are deployed with the app.</span></span> <span data-ttu-id="5b85c-117">O repositório de tempo de execução do .NET Core inclui esses ativos e eles são pré-compilados para melhorar o desempenho.</span><span class="sxs-lookup"><span data-stu-id="5b85c-117">The .NET Core Runtime Store includes these assets, and they are precompiled to improve performance.</span></span> <span data-ttu-id="5b85c-118">Consulte [Metapacote do Microsoft.AspNetCore.All para ASP.NET Core 2.x](xref:fundamentals/metapackage) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="5b85c-118">See [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x](xref:fundamentals/metapackage) for more detail.</span></span>

## <a name="project-structure-differences"></a><span data-ttu-id="5b85c-119">Diferenças de estrutura do projeto</span><span class="sxs-lookup"><span data-stu-id="5b85c-119">Project structure differences</span></span>
<span data-ttu-id="5b85c-120">O formato de arquivo *.csproj* foi simplificado no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5b85c-120">The *.csproj* file format has been simplified in ASP.NET Core.</span></span> <span data-ttu-id="5b85c-121">Algumas alterações importantes incluem:</span><span class="sxs-lookup"><span data-stu-id="5b85c-121">Some notable changes include:</span></span>
- <span data-ttu-id="5b85c-122">A inclusão explícita de arquivos não é necessária para que eles possam ser considerados como parte do projeto.</span><span class="sxs-lookup"><span data-stu-id="5b85c-122">Explicit inclusion of files is not necessary for them to be considered part of the project.</span></span> <span data-ttu-id="5b85c-123">Isso reduz o risco de conflitos de mesclagem XML ao trabalhar em grandes equipes.</span><span class="sxs-lookup"><span data-stu-id="5b85c-123">This reduces the risk of XML merge conflicts when working on large teams.</span></span>
- <span data-ttu-id="5b85c-124">Não há referências baseadas em GUID a outros projetos, o que melhora a legibilidade do arquivo.</span><span class="sxs-lookup"><span data-stu-id="5b85c-124">There are no GUID-based references to other projects, which improves file readability.</span></span>
- <span data-ttu-id="5b85c-125">O arquivo pode ser editado sem descarregá-lo no Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="5b85c-125">The file can be edited without unloading it in Visual Studio:</span></span>

    ![Opção de menu de contexto Editar CSPROJ no Visual Studio 2017](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a><span data-ttu-id="5b85c-127">Substituição do arquivo Global.asax</span><span class="sxs-lookup"><span data-stu-id="5b85c-127">Global.asax file replacement</span></span>
<span data-ttu-id="5b85c-128">O ASP.NET Core introduziu um novo mecanismo para inicializar um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5b85c-128">ASP.NET Core introduced a new mechanism for bootstrapping an app.</span></span> <span data-ttu-id="5b85c-129">O ponto de entrada para aplicativos ASP.NET é o arquivo *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="5b85c-129">The entry point for ASP.NET applications is the *Global.asax* file.</span></span> <span data-ttu-id="5b85c-130">Tarefas como configuração de roteamento e registros de filtro e de área são tratadas no arquivo *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="5b85c-130">Tasks such as route configuration and filter and area registrations are handled in the *Global.asax* file.</span></span>

<span data-ttu-id="5b85c-131">[!code-csharp[Main](samples/globalasax-sample.cs)]</span><span class="sxs-lookup"><span data-stu-id="5b85c-131">[!code-csharp[Main](samples/globalasax-sample.cs)]</span></span>

<span data-ttu-id="5b85c-132">Essa abordagem associa o aplicativo e o servidor no qual ele é implantado de uma forma que interfere na implementação.</span><span class="sxs-lookup"><span data-stu-id="5b85c-132">This approach couples the application and the server to which it's deployed in a way that interferes with the implementation.</span></span> <span data-ttu-id="5b85c-133">Em um esforço para desassociar, a [OWIN](http://owin.org/) foi introduzida para fornecer uma maneira mais limpa de usar várias estruturas juntas.</span><span class="sxs-lookup"><span data-stu-id="5b85c-133">In an effort to decouple, [OWIN](http://owin.org/) was introduced to provide a cleaner way to use multiple frameworks together.</span></span> <span data-ttu-id="5b85c-134">A OWIN fornece um pipeline para adicionar somente os módulos necessários.</span><span class="sxs-lookup"><span data-stu-id="5b85c-134">OWIN provides a pipeline to add only the modules needed.</span></span> <span data-ttu-id="5b85c-135">O ambiente de hospedagem leva uma função [Startup](xref:fundamentals/startup) para configurar serviços e pipeline de solicitação do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5b85c-135">The hosting environment takes a [Startup](xref:fundamentals/startup) function to configure services and the app's request pipeline.</span></span> <span data-ttu-id="5b85c-136">`Startup` registra um conjunto de middleware com o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5b85c-136">`Startup` registers a set of middleware with the application.</span></span> <span data-ttu-id="5b85c-137">Para cada solicitação, o aplicativo chama cada um dos componentes de middleware com o ponteiro de cabeçalho de uma lista vinculada para um conjunto existente de manipuladores.</span><span class="sxs-lookup"><span data-stu-id="5b85c-137">For each request, the application calls each of the the middleware components with the head pointer of a linked list to an existing set of handlers.</span></span> <span data-ttu-id="5b85c-138">Cada componente de middleware pode adicionar um ou mais manipuladores para a pipeline de tratamento de solicitação.</span><span class="sxs-lookup"><span data-stu-id="5b85c-138">Each middleware component can add one or more handlers to the request handling pipeline.</span></span> <span data-ttu-id="5b85c-139">Isso é feito retornando uma referência para o manipulador que é o novo cabeçalho da lista.</span><span class="sxs-lookup"><span data-stu-id="5b85c-139">This is accomplished by returning a reference to the handler that is the new head of the list.</span></span> <span data-ttu-id="5b85c-140">Cada manipulador é responsável por se lembrar do próximo manipulador na lista e por invocá-lo.</span><span class="sxs-lookup"><span data-stu-id="5b85c-140">Each handler is responsible for remembering and invoking the next handler in the list.</span></span> <span data-ttu-id="5b85c-141">Com o ASP.NET Core, o ponto de entrada para um aplicativo é `Startup` e você não tem mais uma dependência de *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="5b85c-141">With ASP.NET Core, the entry point to an application is `Startup`, and you no longer have a dependency on *Global.asax*.</span></span> <span data-ttu-id="5b85c-142">Ao usar a OWIN com o .NET Framework, use algo parecido com o seguinte como um pipeline:</span><span class="sxs-lookup"><span data-stu-id="5b85c-142">When using OWIN with .NET Framework, use something like the following as a pipeline:</span></span>

<span data-ttu-id="5b85c-143">[!code-csharp[Main](samples/webapi-owin.cs)]</span><span class="sxs-lookup"><span data-stu-id="5b85c-143">[!code-csharp[Main](samples/webapi-owin.cs)]</span></span>

<span data-ttu-id="5b85c-144">Isso configura as rotas padrão e usa XmlSerialization em Json por padrão.</span><span class="sxs-lookup"><span data-stu-id="5b85c-144">This configures your default routes, and defaults to XmlSerialization over Json.</span></span> <span data-ttu-id="5b85c-145">Adicione outro Middleware para este pipeline conforme necessário (carregamento de serviços, definições de configuração, arquivos estáticos, etc.).</span><span class="sxs-lookup"><span data-stu-id="5b85c-145">Add other Middleware to this pipeline as needed (loading services, configuration settings, static files, etc.).</span></span>

<span data-ttu-id="5b85c-146">O ASP.NET Core usa uma abordagem semelhante, mas não depende de OWIN para manipular a entrada.</span><span class="sxs-lookup"><span data-stu-id="5b85c-146">ASP.NET Core uses a similar approach, but doesn't rely on OWIN to handle the entry.</span></span> <span data-ttu-id="5b85c-147">Em vez disso, isso é feito por meio do método `Main` de *Program.cs* (semelhante a aplicativos de console) e `Startup` é carregado por lá.</span><span class="sxs-lookup"><span data-stu-id="5b85c-147">Instead, that is done through the *Program.cs* `Main` method (similar to console applications) and `Startup` is loaded through there.</span></span>

<span data-ttu-id="5b85c-148">[!code-csharp[Main](samples/program.cs)]</span><span class="sxs-lookup"><span data-stu-id="5b85c-148">[!code-csharp[Main](samples/program.cs)]</span></span>

<span data-ttu-id="5b85c-149">`Startup` deve incluir um método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="5b85c-149">`Startup` must include a `Configure` method.</span></span> <span data-ttu-id="5b85c-150">Em `Configure`, adicione o middleware necessário ao pipeline.</span><span class="sxs-lookup"><span data-stu-id="5b85c-150">In `Configure`, add the necessary middleware to the pipeline.</span></span> <span data-ttu-id="5b85c-151">No exemplo a seguir (com base no modelo de site da Web padrão), vários métodos de extensão são usados para configurar o pipeline com suporte para:</span><span class="sxs-lookup"><span data-stu-id="5b85c-151">In the following example (from the default web site template), several extension methods are used to configure the pipeline with support for:</span></span>

* [<span data-ttu-id="5b85c-152">BrowserLink</span><span class="sxs-lookup"><span data-stu-id="5b85c-152">BrowserLink</span></span>](http://vswebessentials.com/features/browserlink)
* <span data-ttu-id="5b85c-153">Páginas de erro</span><span class="sxs-lookup"><span data-stu-id="5b85c-153">Error pages</span></span>
* <span data-ttu-id="5b85c-154">Arquivos estáticos</span><span class="sxs-lookup"><span data-stu-id="5b85c-154">Static files</span></span>
* <span data-ttu-id="5b85c-155">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="5b85c-155">ASP.NET Core MVC</span></span>
* <span data-ttu-id="5b85c-156">Identidade</span><span class="sxs-lookup"><span data-stu-id="5b85c-156">Identity</span></span>

<span data-ttu-id="5b85c-157">[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]</span><span class="sxs-lookup"><span data-stu-id="5b85c-157">[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]</span></span>

<span data-ttu-id="5b85c-158">O host e o aplicativo foram separados, o que fornece a flexibilidade de mover para uma plataforma diferente no futuro.</span><span class="sxs-lookup"><span data-stu-id="5b85c-158">The host and application have been decoupled, which provides the flexibility of moving to a different platform in the future.</span></span>

<span data-ttu-id="5b85c-159">**Observação:** para obter uma referência mais aprofundada sobre o Startup do ASP.NET Core e middleware, consulte [Startup no ASP.NET Core](xref:fundamentals/startup)</span><span class="sxs-lookup"><span data-stu-id="5b85c-159">**Note:** For a more in-depth reference to ASP.NET Core Startup and Middleware, see [Startup in ASP.NET Core](xref:fundamentals/startup)</span></span>

## <a name="storing-configurations"></a><span data-ttu-id="5b85c-160">Armazenando configurações</span><span class="sxs-lookup"><span data-stu-id="5b85c-160">Storing Configurations</span></span>
<span data-ttu-id="5b85c-161">O ASP.NET dá suporte ao armazenamento de configurações.</span><span class="sxs-lookup"><span data-stu-id="5b85c-161">ASP.NET supports storing settings.</span></span> <span data-ttu-id="5b85c-162">Essas configurações são usadas, por exemplo, para dar suporte ao ambiente no qual os aplicativos foram implantados.</span><span class="sxs-lookup"><span data-stu-id="5b85c-162">These setting are used, for example, to support the environment to which the applications were deployed.</span></span> <span data-ttu-id="5b85c-163">Uma prática comum era armazenar todos os pares chave-valor personalizados na seção `<appSettings>` do arquivo *Web.config*:</span><span class="sxs-lookup"><span data-stu-id="5b85c-163">A common practice was to store all custom key-value pairs in the `<appSettings>` section of the *Web.config* file:</span></span>

<span data-ttu-id="5b85c-164">[!code-xml[Main](samples/webconfig-sample.xml)]</span><span class="sxs-lookup"><span data-stu-id="5b85c-164">[!code-xml[Main](samples/webconfig-sample.xml)]</span></span>

<span data-ttu-id="5b85c-165">Aplicativos leem essas configurações usando a coleção `ConfigurationManager.AppSettings` no namespace `System.Configuration`:</span><span class="sxs-lookup"><span data-stu-id="5b85c-165">Applications read these settings using the `ConfigurationManager.AppSettings` collection in the `System.Configuration` namespace:</span></span>

<span data-ttu-id="5b85c-166">[!code-csharp[Main](samples/read-webconfig.cs)]</span><span class="sxs-lookup"><span data-stu-id="5b85c-166">[!code-csharp[Main](samples/read-webconfig.cs)]</span></span>

<span data-ttu-id="5b85c-167">O ASP.NET Core pode armazenar dados de configuração para o aplicativo em qualquer arquivo e carregá-los como parte da inicialização de middleware.</span><span class="sxs-lookup"><span data-stu-id="5b85c-167">ASP.NET Core can store configuration data for the application in any file and load them as part of middleware bootstrapping.</span></span> <span data-ttu-id="5b85c-168">O arquivo padrão usado em modelos de projeto é *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="5b85c-168">The default file used in the project templates is *appsettings.json*:</span></span>

<span data-ttu-id="5b85c-169">[!code-json[Main](samples/appsettings-sample.json)]</span><span class="sxs-lookup"><span data-stu-id="5b85c-169">[!code-json[Main](samples/appsettings-sample.json)]</span></span>

<span data-ttu-id="5b85c-170">Carregar esse arquivo em uma instância de `IConfiguration` dentro de seu aplicativo é feito em *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5b85c-170">Loading this file into an instance of `IConfiguration` inside your application is done in *Startup.cs*:</span></span>

<span data-ttu-id="5b85c-171">[!code-csharp[Main](samples/startup-builder.cs)]</span><span class="sxs-lookup"><span data-stu-id="5b85c-171">[!code-csharp[Main](samples/startup-builder.cs)]</span></span>

<span data-ttu-id="5b85c-172">O aplicativo lê de `Configuration` para obter as configurações:</span><span class="sxs-lookup"><span data-stu-id="5b85c-172">The app reads from `Configuration` to get the settings:</span></span>

<span data-ttu-id="5b85c-173">[!code-csharp[Main](samples/read-appsettings.cs)]</span><span class="sxs-lookup"><span data-stu-id="5b85c-173">[!code-csharp[Main](samples/read-appsettings.cs)]</span></span>

<span data-ttu-id="5b85c-174">Existem extensões para essa abordagem para tornar o processo mais robusto, tais como o uso de DI ([injeção de dependência](xref:fundamentals/dependency-injection)) para carregar um serviço com esses valores.</span><span class="sxs-lookup"><span data-stu-id="5b85c-174">There are extensions to this approach to make the process more robust, such as using [Dependency Injection](xref:fundamentals/dependency-injection) (DI) to load a service with these values.</span></span> <span data-ttu-id="5b85c-175">A abordagem de DI fornece um conjunto fortemente tipado de objetos de configuração.</span><span class="sxs-lookup"><span data-stu-id="5b85c-175">The DI approach provides a strongly-typed set of configuration objects.</span></span>

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

<span data-ttu-id="5b85c-176">**Observação:** para uma referência mais detalhada sobre configuração do ASP.NET Core, consulte [Configuração no ASP.NET Core](xref:fundamentals/configuration).</span><span class="sxs-lookup"><span data-stu-id="5b85c-176">**Note:** For a more in-depth reference to ASP.NET Core configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration).</span></span>

## <a name="native-dependency-injection"></a><span data-ttu-id="5b85c-177">Injeção de dependência nativa</span><span class="sxs-lookup"><span data-stu-id="5b85c-177">Native Dependency Injection</span></span>
<span data-ttu-id="5b85c-178">Uma meta importante ao criar aplicativos escalonáveis e grandes é o acoplamento flexível de componentes e serviços.</span><span class="sxs-lookup"><span data-stu-id="5b85c-178">An important goal when building large, scalable applications is the loose coupling of components and services.</span></span> <span data-ttu-id="5b85c-179">A [injeção de dependência](xref:fundamentals/dependency-injection) é uma técnica popular para conseguir isso e é também um componente nativo do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5b85c-179">[Dependency Injection](xref:fundamentals/dependency-injection) is a popular technique for achieving this, and it is a native component of ASP.NET Core.</span></span>

<span data-ttu-id="5b85c-180">Em aplicativos ASP.NET, os desenvolvedores contam com uma biblioteca de terceiros para implementar injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="5b85c-180">In ASP.NET applications, developers rely on a third-party library to implement Dependency Injection.</span></span> <span data-ttu-id="5b85c-181">Um biblioteca desse tipo é a [Unity](https://github.com/unitycontainer/unity), fornecida pelas Diretrizes da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="5b85c-181">One such library is [Unity](https://github.com/unitycontainer/unity), provided by Microsoft Patterns & Practices.</span></span> 

<span data-ttu-id="5b85c-182">Um exemplo de configuração da injeção de dependência com Unity é a implementação de `IDependencyResolver`, que encapsula uma `UnityContainer`:</span><span class="sxs-lookup"><span data-stu-id="5b85c-182">An example of setting up Dependency Injection with Unity is implementing `IDependencyResolver` that wraps a `UnityContainer`:</span></span>

<span data-ttu-id="5b85c-183">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]</span><span class="sxs-lookup"><span data-stu-id="5b85c-183">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]</span></span>

<span data-ttu-id="5b85c-184">Crie uma instância de sua `UnityContainer`, registre seu serviço e defina o resolvedor de dependência de `HttpConfiguration` para a nova instância de `UnityResolver` para o contêiner:</span><span class="sxs-lookup"><span data-stu-id="5b85c-184">Create an instance of your `UnityContainer`, register your service, and set the dependency resolver of `HttpConfiguration` to the new instance of `UnityResolver` for your container:</span></span>

<span data-ttu-id="5b85c-185">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]</span><span class="sxs-lookup"><span data-stu-id="5b85c-185">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]</span></span>

<span data-ttu-id="5b85c-186">Injete `IProductRepository` quando necessário:</span><span class="sxs-lookup"><span data-stu-id="5b85c-186">Inject `IProductRepository` where needed:</span></span>

<span data-ttu-id="5b85c-187">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]</span><span class="sxs-lookup"><span data-stu-id="5b85c-187">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]</span></span>

<span data-ttu-id="5b85c-188">Já que a injeção de dependência é parte do ASP.NET Core, você pode adicionar o serviço no método `ConfigureServices` de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5b85c-188">Because Dependency Injection is part of ASP.NET Core, you can add your service in the `ConfigureServices` method of *Startup.cs*:</span></span>

<span data-ttu-id="5b85c-189">[!code-csharp[Main](samples/configure-services.cs)]</span><span class="sxs-lookup"><span data-stu-id="5b85c-189">[!code-csharp[Main](samples/configure-services.cs)]</span></span>

<span data-ttu-id="5b85c-190">O repositório pode ser injetado em qualquer lugar, como ocorria com a Unity.</span><span class="sxs-lookup"><span data-stu-id="5b85c-190">The repository can be injected anywhere, as was true with Unity.</span></span>

<span data-ttu-id="5b85c-191">**Observação:** para uma referência detalhada sobre a injeção de dependência no ASP.NET Core, consulte [Injeção de dependência no ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span><span class="sxs-lookup"><span data-stu-id="5b85c-191">**Note:** For an in-depth reference to dependency injection in ASP.NET Core, see [Dependency Injection in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="5b85c-192">Servir arquivos estáticos</span><span class="sxs-lookup"><span data-stu-id="5b85c-192">Serving Static Files</span></span>
<span data-ttu-id="5b85c-193">Uma parte importante do desenvolvimento da Web é a capacidade de servir ativos estáticos, do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="5b85c-193">An important part of web development is the ability to serve static, client-side assets.</span></span> <span data-ttu-id="5b85c-194">Os exemplos mais comuns de arquivos estáticos são HTML, CSS, Javascript e imagens.</span><span class="sxs-lookup"><span data-stu-id="5b85c-194">The most common examples of static files are HTML, CSS, Javascript, and images.</span></span> <span data-ttu-id="5b85c-195">Esses arquivos precisam ser salvos no local de publicação do aplicativo (ou CDN) e referenciados para que eles possam ser carregados por uma solicitação.</span><span class="sxs-lookup"><span data-stu-id="5b85c-195">These files need to be saved in the published location of the app (or CDN) and referenced so they can be loaded by a request.</span></span> <span data-ttu-id="5b85c-196">Esse processo foi alterado no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5b85c-196">This process has changed in ASP.NET Core.</span></span>

<span data-ttu-id="5b85c-197">No ASP.NET, arquivos estáticos são armazenados em vários diretórios e referenciados nas exibições.</span><span class="sxs-lookup"><span data-stu-id="5b85c-197">In ASP.NET, static files are stored in various directories and referenced in the views.</span></span>

<span data-ttu-id="5b85c-198">No ASP.NET Core, arquivos estáticos são armazenados na "raiz da Web" (*&lt;raiz do conteúdo&gt;/wwwroot*), a menos que configurado de outra forma.</span><span class="sxs-lookup"><span data-stu-id="5b85c-198">In ASP.NET Core, static files are stored in the "web root" (*&lt;content root&gt;/wwwroot*), unless configured otherwise.</span></span> <span data-ttu-id="5b85c-199">Os arquivos são carregados no pipeline de solicitação invocando o método de extensão `UseStaticFiles` de `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="5b85c-199">The files are loaded into the request pipeline by invoking the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

<span data-ttu-id="5b85c-200">[!code-csharp[Main](../../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="5b85c-200">[!code-csharp[Main](../../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]</span></span>

<span data-ttu-id="5b85c-201">**Observação**: se você usar o .NET Framework como destino, instale o pacote NuGet `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="5b85c-201">**Note:** If targeting .NET Framework, install the NuGet package `Microsoft.AspNetCore.StaticFiles`.</span></span>

<span data-ttu-id="5b85c-202">Por exemplo, um ativo de imagem na pasta *wwwroot/imagens* está acessível para o navegador em um local como `http://<app>/images/<imageFileName>`.</span><span class="sxs-lookup"><span data-stu-id="5b85c-202">For example, an image asset in the *wwwroot/images* folder is accessible to the browser at a location such as `http://<app>/images/<imageFileName>`.</span></span>

<span data-ttu-id="5b85c-203">**Observação:** para obter uma referência mais aprofundada sobre como servir arquivos estáticos no ASP.NET Core, consulte [Introdução ao trabalho com arquivos estáticos no ASP.NET Core](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="5b85c-203">**Note:** For a more in-depth reference to serving static files in ASP.NET Core, see [Introduction to working with static files in ASP.NET Core](xref:fundamentals/static-files).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5b85c-204">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="5b85c-204">Additional Resources</span></span>
* [<span data-ttu-id="5b85c-205">Fazendo a portabilidade de Bibliotecas para o .NET Core</span><span class="sxs-lookup"><span data-stu-id="5b85c-205">Porting Libraries to .NET Core</span></span>](https://docs.microsoft.com/dotnet/core/porting/libraries)