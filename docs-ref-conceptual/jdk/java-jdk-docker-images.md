---
title: Azure Java 開発用の JDK で Docker イメージを使用する
description: ''
author: bmitchell287
manager: douge
ms.author: brendm
ms.date: 4/9/2019
ms.devlang: java
ms.topic: conceptual
ms.openlocfilehash: ee8df2a08b23d090965cb42e2c15b934d4785e7c
ms.sourcegitcommit: 03379369346974c6e80f86e7129b885112b5c1a9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2019
ms.locfileid: "64910229"
---
# <a name="use-docker-with-a-jdk-for-azure"></a><span data-ttu-id="aaeb1-102">Azure 用の JDK で Docker を使用する</span><span class="sxs-lookup"><span data-stu-id="aaeb1-102">Use Docker with a JDK for Azure</span></span> 

<span data-ttu-id="aaeb1-103">Java 7、8、11 用のビルド済み Docker イメージは [Docker Hub](https://hub.docker.com/_/microsoft-java-se) から利用できます。</span><span class="sxs-lookup"><span data-stu-id="aaeb1-103">Pre-built Docker images for Java 7, 8, and 11 are available through [Docker Hub](https://hub.docker.com/_/microsoft-java-se).</span></span>

<span data-ttu-id="aaeb1-104">複数のベース OS イメージ上の Zulu JDK、JRE、および JRE ヘッドレス用の認定済み Docker コンテナー イメージを Docker Hub で利用できます。</span><span class="sxs-lookup"><span data-stu-id="aaeb1-104">Certified Docker container images for Zulu JDK, JRE, and JRE-headless on multiple base OS images are available at Docker Hub:</span></span>

* [<span data-ttu-id="aaeb1-105">JDK</span><span class="sxs-lookup"><span data-stu-id="aaeb1-105">JDK</span></span>](https://hub.docker.com/_/microsoft-java-jdk)
* [<span data-ttu-id="aaeb1-106">JRE</span><span class="sxs-lookup"><span data-stu-id="aaeb1-106">JRE</span></span>](https://hub.docker.com/_/microsoft-java-jre)
* [<span data-ttu-id="aaeb1-107">JRE ヘッドレス</span><span class="sxs-lookup"><span data-stu-id="aaeb1-107">JRE-headless</span></span>](https://hub.docker.com/_/microsoft-java-jre-headless)

## <a name="running-a-docker-image"></a><span data-ttu-id="aaeb1-108">Docker イメージの実行</span><span class="sxs-lookup"><span data-stu-id="aaeb1-108">Running a Docker image</span></span>

<span data-ttu-id="aaeb1-109">次の例に示すように、Docker イメージは構文 `$ docker run mcr.microsoft.com/java/jdk:tag java` を使用して実行できます。</span><span class="sxs-lookup"><span data-stu-id="aaeb1-109">Docker images can be run using the syntax `$ docker run mcr.microsoft.com/java/jdk:tag java` as shown in the following example.</span></span>

```cli
docker run mcr.microsoft.com/java/jdk:8u212-zulu-alpine java -version 
```

## <a name="creating-a-docker-image"></a><span data-ttu-id="aaeb1-110">Docker イメージの作成</span><span class="sxs-lookup"><span data-stu-id="aaeb1-110">Creating a Docker image</span></span>

<span data-ttu-id="aaeb1-111">次の例に示すように、Microsoft の公式 Docker Hub イメージを使用してイメージを作成できます。</span><span class="sxs-lookup"><span data-stu-id="aaeb1-111">You can create an image using Microsoft's official Docker Hub images as shown in the following examples.</span></span>

### <a name="create-a-docker-file"></a><span data-ttu-id="aaeb1-112">Docker ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="aaeb1-112">Create a Docker file</span></span>

```cli
FROM mcr.microsoft.com/java/jdk:8u212-zulu-alpine 
  
RUN echo $' \
  
public class HelloWorld { \
   public static void main(String[] args) { \
      // Prints "Hello, World" in the terminal window. \
      System.out.println("Hello, World - From Microsoft Azure !!!"); \
   } \
}' > HelloWorld.java
  
RUN javac HelloWorld.java
  
CMD ["java", "HelloWorld"]
```

### <a name="build-a-docker-image"></a><span data-ttu-id="aaeb1-113">Docker イメージをビルドする</span><span class="sxs-lookup"><span data-stu-id="aaeb1-113">Build a Docker image</span></span>

```cli
docker build -t hello-world
```

### <a name="run-the-new-image"></a><span data-ttu-id="aaeb1-114">新しいイメージを実行する</span><span class="sxs-lookup"><span data-stu-id="aaeb1-114">Run the new image</span></span>

```cli
docker run hello-world
```

<span data-ttu-id="aaeb1-115">次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="aaeb1-115">You will see the following output:</span></span>

```output
Hello World - From Microsoft Azure !!!
```
