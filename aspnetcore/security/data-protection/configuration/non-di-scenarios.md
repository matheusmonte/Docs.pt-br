---
title: "Cenários com suporte a DI não"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a7d8a962-80ff-48e3-96f6-8472b7ba2df9
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 54a930c26f9f48ea0e6f7865e2927bcde0f4d6c0
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="non-di-aware-scenarios"></a><span data-ttu-id="5dbdf-103">Cenários com suporte a DI não</span><span class="sxs-lookup"><span data-stu-id="5dbdf-103">Non DI aware scenarios</span></span>

<span data-ttu-id="5dbdf-104">O sistema de proteção de dados normalmente é projetado [a ser adicionado a um contêiner de serviço](../consumer-apis/overview.md) e a ser fornecido para os componentes dependentes por meio de um mecanismo de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="5dbdf-104">The data protection system is normally designed [to be added to a service container](../consumer-apis/overview.md) and to be provided to dependent components via a DI mechanism.</span></span> <span data-ttu-id="5dbdf-105">No entanto, pode haver alguns casos em que isso não é viável, especialmente ao importar o sistema para um aplicativo existente.</span><span class="sxs-lookup"><span data-stu-id="5dbdf-105">However, there may be some cases where this is not feasible, especially when importing the system into an existing application.</span></span>

<span data-ttu-id="5dbdf-106">Para dar suporte a esses cenários o pacote Microsoft.AspNetCore.DataProtection.Extensions fornece um tipo concreto DataProtectionProvider que oferece uma maneira simples de usar o sistema de proteção de dados sem passar pelo caminhos de código específico de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="5dbdf-106">To support these scenarios the package Microsoft.AspNetCore.DataProtection.Extensions provides a concrete type DataProtectionProvider which offers a simple way to use the data protection system without going through DI-specific code paths.</span></span> <span data-ttu-id="5dbdf-107">O próprio tipo implementa IDataProtectionProvider e criá-la é tão fácil quanto fornecer um DirectoryInfo onde as chaves de criptografia do provedor devem ser armazenadas.</span><span class="sxs-lookup"><span data-stu-id="5dbdf-107">The type itself implements IDataProtectionProvider, and constructing it is as easy as providing a DirectoryInfo where this provider's cryptographic keys should be stored.</span></span>

<span data-ttu-id="5dbdf-108">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5dbdf-108">For example:</span></span>

<span data-ttu-id="5dbdf-109">[!code-none[Main](non-di-scenarios/_static/nodisample1.cs)]</span><span class="sxs-lookup"><span data-stu-id="5dbdf-109">[!code-none[Main](non-di-scenarios/_static/nodisample1.cs)]</span></span>

>[!WARNING]
> <span data-ttu-id="5dbdf-110">Por padrão o DataProtectionProvider tipo concreto não criptografa material de chave bruto persisti-los antes do sistema de arquivos.</span><span class="sxs-lookup"><span data-stu-id="5dbdf-110">By default the DataProtectionProvider concrete type does not encrypt raw key material before persisting it to the file system.</span></span> <span data-ttu-id="5dbdf-111">Isso é para dar suporte a cenários onde os pontos de desenvolvedor para uma rede de compartilham, caso em que o sistema de proteção de dados não é possível deduzir automaticamente um mecanismo de criptografia de chave apropriado em repouso.</span><span class="sxs-lookup"><span data-stu-id="5dbdf-111">This is to support scenarios where the developer points to a network share, in which case the data protection system cannot automatically deduce an appropriate at-rest key encryption mechanism.</span></span>
>
><span data-ttu-id="5dbdf-112">Além disso, o tipo concreto DataProtectionProvider não [isolar aplicativos](overview.md#data-protection-configuration-per-app-isolation) por padrão, para que todos os aplicativos apontados para o mesmo diretório chave podem compartilhar cargas como sua finalidade correspondem.</span><span class="sxs-lookup"><span data-stu-id="5dbdf-112">Additionally, the DataProtectionProvider concrete type does not [isolate applications](overview.md#data-protection-configuration-per-app-isolation) by default, so all applications pointed at the same key directory can share payloads as long as their purpose parameters match.</span></span>

<span data-ttu-id="5dbdf-113">O desenvolvedor do aplicativo pode atender a ambas se desejado.</span><span class="sxs-lookup"><span data-stu-id="5dbdf-113">The application developer can address both of these if desired.</span></span> <span data-ttu-id="5dbdf-114">O construtor DataProtectionProvider aceita um [retorno de chamada de configuração opcional](overview.md#data-protection-configuration-callback) que pode ser usado para ajustar os comportamentos do sistema.</span><span class="sxs-lookup"><span data-stu-id="5dbdf-114">The DataProtectionProvider constructor accepts an [optional configuration callback](overview.md#data-protection-configuration-callback) which can be used to tweak the behaviors of the system.</span></span> <span data-ttu-id="5dbdf-115">O exemplo a seguir demonstra a restauração isolamento por meio de uma chamada explícita para SetApplicationName e também demonstra como configurar o sistema para criptografar automaticamente chaves persistentes usando Windows DPAPI.</span><span class="sxs-lookup"><span data-stu-id="5dbdf-115">The sample below demonstrates restoring isolation via an explicit call to SetApplicationName, and it also demonstrates configuring the system to automatically encrypt persisted keys using Windows DPAPI.</span></span> <span data-ttu-id="5dbdf-116">Se o diretório aponta para um compartilhamento UNC, você poderá distribuir um certificado compartilhado por todos os computadores relevantes e para configurar o sistema para usar a criptografia baseada em certificado em vez disso, por meio de uma chamada para [ProtectKeysWithCertificate](overview.md#configuring-x509-certificate).</span><span class="sxs-lookup"><span data-stu-id="5dbdf-116">If the directory points to a UNC share, you may wish to distribute a shared certificate across all relevant machines and to configure the system to use certificate-based encryption instead via a call to [ProtectKeysWithCertificate](overview.md#configuring-x509-certificate).</span></span>

<span data-ttu-id="5dbdf-117">[!code-none[Main](non-di-scenarios/_static/nodisample2.cs)]</span><span class="sxs-lookup"><span data-stu-id="5dbdf-117">[!code-none[Main](non-di-scenarios/_static/nodisample2.cs)]</span></span>

>[!TIP]
> <span data-ttu-id="5dbdf-118">Instâncias do tipo concreto DataProtectionProvider são caras de criar.</span><span class="sxs-lookup"><span data-stu-id="5dbdf-118">Instances of the DataProtectionProvider concrete type are expensive to create.</span></span> <span data-ttu-id="5dbdf-119">Se um aplicativo mantém várias instâncias desse tipo e se eles está apontado no mesmo diretório de armazenamento de chaves, o desempenho do aplicativo pode ser degradado.</span><span class="sxs-lookup"><span data-stu-id="5dbdf-119">If an application maintains multiple instances of this type and if they're all pointing at the same key storage directory, application performance may be degraded.</span></span> <span data-ttu-id="5dbdf-120">O uso pretendido é que o desenvolvedor do aplicativo instanciar esse tipo de uma vez, em seguida, mantenha a reutilizar essa referência único tanto quanto possível.</span><span class="sxs-lookup"><span data-stu-id="5dbdf-120">The intended usage is that the application developer instantiate this type once then keep reusing this single reference as much as possible.</span></span> <span data-ttu-id="5dbdf-121">O tipo de DataProtectionProvider e todas as instâncias de IDataProtector criadas a partir dele são thread-safe para chamadores vários.</span><span class="sxs-lookup"><span data-stu-id="5dbdf-121">The DataProtectionProvider type and all IDataProtector instances created from it are thread-safe for multiple callers.</span></span>