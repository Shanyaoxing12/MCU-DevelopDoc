# **Getting Started with Silicon Labs Thread** <!-- omit in toc -->

该快速入门指南提供了使用 Silicon Labs Thread 协议栈（version 2.4 及更高版本）和 Simplicity Studio（version 4）为 Mighty Gecko（EFR32MG）系列 SoCs 配置，构建和安装应用程序的基本信息。

本指南适用于 Silicon Labs Thread 和 Silicon Labs 硬件开发的新手。它说明了如何开始使用 Silicon Labs Thread 协议栈提供的示例应用程序。

关键点：
* 产品概述
* 设置开发环境
* 安装 Simplicity Studio 和 Silicon Labs Thread
* 创建一个示例应用网络
* 使用网络分析仪

PS: 下表为一些译词及其英文原型的对应表

|  译词  |  原型  |
|  :----:  |  :----:  |
| 协议栈 | stack |

------------------------------------------------------------------------------------------------------------------------


- [**1. 产品概述(Product Overview)**](#1-产品概述product-overview)
    - [**1.1 软件组件(Software Components)**](#11-软件组件software-components)
    - [**1.2 支持(Support)**](#12-支持support)
    - [**1.3 文档(Documentation)**](#13-文档documentation)


# **1. 产品概述(Product Overview)**

在跟随本指南中的步骤前，你必须：
* 已购买 Mighty Gecko (EFR32MG) Mesh Networking Kit（参见 http://www.silabs.com/products/wireless/mesh-networking/thread/Pages/thread.aspx 以获取更多信息）
* 下载如下所述的必要软件组件。您的开发硬件套件中包含一张卡，该卡中有一个链接指向一个入门页面，入门页面将引导您访问 Silicon Labs 的软件产品链接。

## **1.1 软件组件(Software Components)**

有关协议栈和这些组件的版本限制和兼容约束，请参阅协议栈发行说明。

要开发 Silicon Labs Thread 应用程序，您将需要以下内容。安装说明在 [2.3 安装 Simplicity Studio 和 Silicon Labs Thread]() 小节中提供：
* 整合了 AppBuilder 的 Simplicity Studio（version 4）开发环境。如果你没有，请连接到 http://www.silabs.com/products/mcu/Pages/simplicity-studio-v4.aspx 以下载它。AppBuilder 是一个交互式 GUI 工具，它允许您配置由 Silicon lab 提供的代码主体来实现应用程序。AppBuilder 和其他 Simple Studio 模块可在线提供帮助。
* Silicon Labs Thread 协议栈是一个无线协议栈的高级实现，其通过 Simple Studio 安装。协议栈 API 记录在在线 API 参考以及通过 Simplicity Studio 获得的其他文档中。协议栈以库集合的形式提供，您可以将这些库链接到您的应用程序。开发环境中提供了对每个库的描述。发行说明包含了已安装文件夹及其内容的详细信息。
* Simplicity Commander，随 Simplicity Studio 一起安装。一个功能有限的 GUI，可以通过 Simplicity Studio 的工具菜单访问它。大部分功能可以通过在 Simplicity Commander 目录（**\\SiliconLabs\\SimplicityStudio\\v4\\developer\\adapter_packs\\commander**）中打开命令提示符调用 CLI 进行访问。有关更多信息，请参阅  **UG162: Simplicity Commander Reference Guide** 。
* 编译工具链
    * Simple Studio 提供了 GCC（GNU 编译器集合）。本文档中使用 GCC。
    > 注意：用 GCC 创建的应用程序映像比用 IAR 创建的要大。如果您使用 GCC 来编译这个 SDK 中的示例应用程序，那么您必须使用至少 512kb 的 flash。
    * IAR Embedded Workbench for ARM（IAR-EWARM）（可选）
    > 注意：请参阅该版本的 Silicon Labs Thread SDK 的发行说明中支持的 IAR 版本。从 Silicon Labs 支持入口下载支持的版本，如 [2.3 安装 Simplicity Studio 和 Silicon Labs Thread]() 末尾所述。有关安装过程以及如何配置许可证的额外信息，请参阅 IAR 安装程序的 “快速入门安装信息” 部分。安装 IAR-EWARM 后，下次 Simplicity Studio 启动时，它将自动检测并配置 IDE 以使用 IAR-EWARM。

虽然 Simplicity Studio 和 Simplicity Commander 可以在 Mac OS 或 Linux 机器上运行，但这些说明是假设您正在使用基于 Microsoft Windows 的 PC。如果您使用的是非 Windows 系统，则必须通过 WINE 或其他形式的模拟器或虚拟机运行 IAR-EWARM。

## **1.2 支持(Support)**

您可以通过 Simplicity Studio 的 "资源" 选项卡访问位于 https://www.silabs.com/support 的 Silicon Labs 支持入口，如 [3.4 访问文档和其他资源]() 中所述。如果您在开发期间有任何疑问，可以使用该入口联系客户支持。

## **1.3 文档(Documentation)**

可以通过 Simplicity Studio 访问协议栈文档，如 [3.4 访问文档和其他资源]() 中所述。Simplicity Studio 还提供了硬件文档和其他应用笔记的链接。有关 Silicon Labs Thread 软件的更多详细信息，请参阅发行说明。

------------------------------------------------------------------------------------------------------------------------
