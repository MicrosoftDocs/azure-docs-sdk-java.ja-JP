---
ms.openlocfilehash: ab31ee32ea940db2d7bcfa2fe36475d8a648bfc9
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592615"
---
| <span data-ttu-id="e28c5-101">**仮想マシンの作成**</span><span class="sxs-lookup"><span data-stu-id="e28c5-101">**Create virtual machines**</span></span> || 
|---|---|
| <span data-ttu-id="e28c5-102">[仮想マシンを管理する][1]</span><span class="sxs-lookup"><span data-stu-id="e28c5-102">[Manage virtual machines][1]</span></span> | <span data-ttu-id="e28c5-103">仮想マシンを作成、変更、開始、停止、削除します。</span><span class="sxs-lookup"><span data-stu-id="e28c5-103">Create, modify, start, stop, and delete virtual machines.</span></span> |
| <span data-ttu-id="e28c5-104">[カスタム イメージから仮想マシンを作成する][2]</span><span class="sxs-lookup"><span data-stu-id="e28c5-104">[Create a virtual machine from a custom image][2]</span></span> | <span data-ttu-id="e28c5-105">仮想マシンのカスタム イメージを作成し、そのイメージを使用して新しい仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="e28c5-105">Create a custom virtual machine image and use it to create new virtual machines.</span></span> | 
| <span data-ttu-id="e28c5-106">[スナップショットから特殊化された VHD を使用して仮想マシンを作成する][3]</span><span class="sxs-lookup"><span data-stu-id="e28c5-106">[Create a virtual machine using specialized VHD from a snapshot][3]</span></span> | <span data-ttu-id="e28c5-107">仮想マシンの OS とデータ ディスクからスナップショットを作成し、そのスナップショットからマネージド ディスクを作成した後、マネージド ディスクをアタッチすることにより仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="e28c5-107">Create snapshot from the virtual machine's OS and data disks, create managed disks from the snapshots, and then create a virtual machine by attaching the managed disks.</span></span> |  
| <span data-ttu-id="e28c5-108">[同一ネットワーク内に同時に仮想マシンを作成する][4]</span><span class="sxs-lookup"><span data-stu-id="e28c5-108">[Create virtual machines in parallel in the same network][4]</span></span> | <span data-ttu-id="e28c5-109">2 つのサブネットがある同一仮想ネットワーク上の同一リージョンに、並行して複数の仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="e28c5-109">Create virtual machines in the same region on the same virtual network with two subnets in parallel.</span></span> |
| <span data-ttu-id="e28c5-110">[複数のリージョンに対して同時に仮想マシンを作成する][5]</span><span class="sxs-lookup"><span data-stu-id="e28c5-110">[Create virtual machines across regions in parallel][5]</span></span> | <span data-ttu-id="e28c5-111">複数の Azure リージョンにわたって一連の仮想マシンを作成し、負荷分散します。</span><span class="sxs-lookup"><span data-stu-id="e28c5-111">Create and load-balance a set of virtual machines across multiple Azure regions.</span></span> |
| <span data-ttu-id="e28c5-112">**ネットワークの仮想マシン**</span><span class="sxs-lookup"><span data-stu-id="e28c5-112">**Network virtual machines**</span></span> || 
| <span data-ttu-id="e28c5-113">[仮想ネットワークを管理する][6]</span><span class="sxs-lookup"><span data-stu-id="e28c5-113">[Manage virtual networks][6]</span></span> | <span data-ttu-id="e28c5-114">2 つのサブネットがある仮想ネットワークを設定し、これらに対するインターネット アクセスを制限します。</span><span class="sxs-lookup"><span data-stu-id="e28c5-114">Set up a virtual network with two subnets and restrict Internet access to them.</span></span> |
| <span data-ttu-id="e28c5-115">**スケール セットの作成**</span><span class="sxs-lookup"><span data-stu-id="e28c5-115">**Create scale sets**</span></span> ||
| <span data-ttu-id="e28c5-116">[ロード バランサーを使って仮想マシン スケール セットを作成する][7]</span><span class="sxs-lookup"><span data-stu-id="e28c5-116">[Create a virtual machine scale set with a load balancer][7]</span></span> | <span data-ttu-id="e28c5-117">VM スケール セットを作成してロード バランサーを設定し、スケール セットの VM への SSH 接続文字列を取得します。</span><span class="sxs-lookup"><span data-stu-id="e28c5-117">Create a VM scale set, set up a load balancer, and get SSH connection strings to the scale set VMs.</span></span> |

[1]: ../java-sdk-manage-virtual-machines.md
[2]: https://azure.microsoft.com/resources/samples/managed-disk-java-create-virtual-machine-using-custom-image/
[3]: https://azure.microsoft.com/resources/samples/managed-disk-java-create-virtual-machine-using-specialized-disk-from-vhd/
[4]: https://azure.microsoft.com/resources/samples/compute-java-manage-virtual-machines-in-parallel/
[5]: ../java-sdk-virtual-machines-in-parallel.md
[6]: ../java-sdk-manage-virtual-networks.md
[7]: ../java-sdk-manage-vm-scalesets.md