# **Getting Started with Silicon Labs Thread** <!-- omit in toc -->

该快速入门指南提供了使用 Silicon Labs Thread stack（version 2.4 及更高版本）和 Simplicity Studio（version 4）为 Mighty Gecko（EFR32MG）系列 SoCs 配置，构建和安装应用程序的基本信息。

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
- [**2. 设置开发环境(Setting Up Your Development Environment)**](#2-设置开发环境setting-up-your-development-environment)
    - [**2.1 连接无线入门套件(Connect the WSTK)**](#21-连接无线入门套件connect-the-wstk)
    - [**2.2 注册开发套件(Register your Development Kit)**](#22-注册开发套件register-your-development-kit)
    - [**2.3 安装 Simplicity Studio 和 Silicon Labs Thread(Install Simplicity Studio and the Silicon Labs Thread Stack)**](#23-安装-simplicity-studio-和-silicon-labs-threadinstall-simplicity-studio-and-the-silicon-labs-thread-stack)
- [**3. 启动程序透视图中的功能(Functionality in the Launcher Perspective)**](#3-启动程序透视图中的功能functionality-in-the-launcher-perspective)
    - [**3.1 下载更新或安装额外组件(Downloading Updates or Installing Additional Components)**](#31-下载更新或安装额外组件downloading-updates-or-installing-additional-components)
    - [**3.2 更改首选的 SDK(Changing the Preferred SDK)**](#32-更改首选的-sdkchanging-the-preferred-sdk)
    - [**3.3 更新适配器固件(Updating Adapter Firmware)**](#33-更新适配器固件updating-adapter-firmware)
    - [**3.4 访问文档和其他资源(Accessing Documentation and Other Resources)**](#34-访问文档和其他资源accessing-documentation-and-other-resources)
- [**4. 关于演示和示例(About Demos and Examples)**](#4-关于演示和示例about-demos-and-examples)
    - [**4.1 演示(Demos)**](#41-演示demos)
    - [**4.2 软件示例(Software Examples)**](#42-软件示例software-examples)
    - [**4.3 从一个空白应用程序开始(Starting with a Blank Application)**](#43-从一个空白应用程序开始starting-with-a-blank-application)
- [**5. 应用程序开发入门(Getting Started with Application Development)**](#5-应用程序开发入门getting-started-with-application-development)
    - [**5.1 选择一个示例应用(Selecting an Example Application)**](#51-选择一个示例应用selecting-an-example-application)
    - [**5.2 生成应用程序源文件(Generating the Application Source Files)**](#52-生成应用程序源文件generating-the-application-source-files)
    - [**5.3 编译和刷新应用程序(Compiling and Flashing the Application)**](#53-编译和刷新应用程序compiling-and-flashing-the-application)
        - [**5.3.1 关于 Bootloading(About Bootloading)**](#531-关于-bootloadingabout-bootloading)
        - [**5.3.2 构建和刷新文件(Building and Flashing Files)**](#532-构建和刷新文件building-and-flashing-files)
- [**6. 创建网络(Creating a Network)**](#6-创建网络creating-a-network)
- [**7. 使用网络分析仪(Using the Network Analyzer)**](#7-使用网络分析仪using-the-network-analyzer)
- [**8. 下一步(Next Steps)**](#8-下一步next-steps)


# **1. 产品概述(Product Overview)**

在跟随本指南中的步骤前，你必须：
* 已购买 Mighty Gecko（EFR32MG）Mesh Networking Kit（参见 http://www.silabs.com/products/wireless/mesh-networking/thread/Pages/thread.aspx 以获取更多信息）。
* 下载如下所述的必要软件组件。您的开发硬件套件中包含一张卡，该卡中有一个链接指向一个入门页面，入门页面将引导您访问 Silicon Labs 的软件产品链接。

## **1.1 软件组件(Software Components)**

有关协议栈和这些组件的版本限制和兼容约束，请参阅协议栈发行说明。

要开发 Silicon Labs Thread 应用程序，您将需要以下内容。安装说明在 [2.3 安装 Simplicity Studio 和 Silicon Labs Thread](#23-安装-simplicity-studio-和-silicon-labs-threadinstall-simplicity-studio-and-the-silicon-labs-thread-stack) 小节中提供：
* 整合了 AppBuilder 的 Simplicity Studio（version 4）开发环境。如果你没有，请连接到 http://www.silabs.com/products/mcu/Pages/simplicity-studio-v4.aspx 以下载它。AppBuilder 是一个交互式 GUI 工具，它允许您配置由 Silicon lab 提供的代码主体来实现应用程序。AppBuilder 和其他 Simple Studio 模块可在线提供帮助。
* Silicon Labs Thread 协议栈是一个无线协议栈的高级实现，其通过 Simple Studio 安装。协议栈 API 记录在在线 API 参考以及通过 Simplicity Studio 获得的其他文档中。协议栈以库集合的形式提供，您可以将这些库链接到您的应用程序。开发环境中提供了对每个库的描述。发行说明包含了已安装文件夹及其内容的详细信息。
* Simplicity Commander，随 Simplicity Studio 一起安装。一个功能有限的 GUI，可以通过 Simplicity Studio 的工具菜单访问它。大部分功能可以通过在 Simplicity Commander 目录（**\\SiliconLabs\\SimplicityStudio\\v4\\developer\\adapter_packs\\commander**）中打开命令提示符调用 CLI 进行访问。有关更多信息，请参阅  **UG162: Simplicity Commander Reference Guide** 。
* 编译工具链
    * Simple Studio 提供了 GCC（GNU 编译器集合）。本文档中使用 GCC。
    > 注意：用 GCC 创建的应用程序映像比用 IAR 创建的要大。如果您使用 GCC 来编译这个 SDK 中的示例应用程序，那么您必须使用至少 512kb 的 flash。
    * IAR Embedded Workbench for ARM（IAR-EWARM）（可选）
    > 注意：请参阅该版本的 Silicon Labs Thread SDK 的发行说明中支持的 IAR 版本。从 Silicon Labs 支持入口下载支持的版本，如 [2.3 安装 Simplicity Studio 和 Silicon Labs Thread](#23-安装-simplicity-studio-和-silicon-labs-threadinstall-simplicity-studio-and-the-silicon-labs-thread-stack) 末尾所述。有关安装过程以及如何配置许可证的额外信息，请参阅 IAR 安装程序的 “快速入门安装信息” 部分。安装 IAR-EWARM 后，下次 Simplicity Studio 启动时，它将自动检测并配置 IDE 以使用 IAR-EWARM。

虽然 Simplicity Studio 和 Simplicity Commander 可以在 Mac OS 或 Linux 机器上运行，但这些说明是假设您正在使用基于 Microsoft Windows 的 PC。如果您使用的是非 Windows 系统，则必须通过 WINE 或其他形式的模拟器或虚拟机运行 IAR-EWARM。

## **1.2 支持(Support)**

您可以通过 Simplicity Studio 的 Resources 选项卡访问位于 https://www.silabs.com/support 的 Silicon Labs 支持入口，如 [3.4 访问文档和其他资源](#34-访问文档和其他资源accessing-documentation-and-other-resources) 中所述。如果您在开发期间有任何疑问，可以使用该入口联系客户支持。

## **1.3 文档(Documentation)**

可以通过 Simplicity Studio 访问协议栈文档，如 [3.4 访问文档和其他资源](#34-访问文档和其他资源accessing-documentation-and-other-resources) 中所述。Simplicity Studio 还提供了硬件文档和其他应用笔记的链接。有关 Silicon Labs Thread 软件的更多详细信息，请参阅发行说明。

------------------------------------------------------------------------------------------------------------------------

# **2. 设置开发环境(Setting Up Your Development Environment)**

## **2.1 连接无线入门套件(Connect the WSTK)**

将无线板安装好，并通过 USB 电缆将 WSTK 连接到您的 PC 上。若在 Simplicity Studio 已安装后连接它，Simplicity Studio 将自动获得它需要的相关额外资源。

> 注意：为了在 Simple Studio 中获得最佳性能，请确保 WSTK 上的电源开关处于高级能源监测或 “AEM” 位置，如下图所示。

![Figure 1. EFR32MG12 on a WSTK](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-F1.jpg)

## **2.2 注册开发套件(Register your Development Kit)**

在安装 Simplicity Studio 之前，您需要在支持入口上创建一个帐户。请务必记录您的帐户用户名和密码，因为您将使用它来登录 Simplicity Studio。要从 Simplicity Studio 中安装 Silicon Labs Thread 协议栈，您还必须使用 Mighty Gecko Kit 序列号在 https://siliconlabs.force.com/KitRegistration 上注册您的套件。如果您愿意，可以在安装期间通过 Simplicity Studio 注册您的套件。

## **2.3 安装 Simplicity Studio 和 Silicon Labs Thread(Install Simplicity Studio and the Silicon Labs Thread Stack)**

1. 运行 Simplicity Studio 安装程序。
2. 当 “Simplicity Studio” 安装程序首次启动时，它会显示一个许可协议对话框。接受协议条款并单击 Next \>。
    
    ![2.3 p1](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-2.3p1.jpg)

3. 选择目标位置，单击 Next \>，然后单击 Install。
4. 当应用程序启动时，会邀请您登录。使用您的支持帐户用户名和密码登录。虽然您可以在这里跳过登录，但是您必须登录并注册您的开发套件包，才能下载受保护的协议栈，比如 Silicon Labs Thread。
    
    ![2.3 p2](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-2.3p2.jpg)

5. 登录后，Simplicity Studio 添加软件信息。一旦初始软件安装完成，Simplicity Studio 将检查连接的硬件。如果有 WSTK 通过 USB 电缆连接上，Simplicity Studio 将检测 USB 电缆，并提示你下载设备检查器。单击 Yes。
    
    ![2.3 p3](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-2.3p3.jpg)

6. 在一些额外项安装后，您可以选择通过设备安装（步骤8和9）或通过产品组安装（步骤7）。
    
    ![2.3 p4](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-2.3p4.jpg)

    在这些过程中，您可以随时单击 Home 按钮返回到此对话框。
7.  如果您单击 “Install by Product Group”，您将获得产品组列表。点击你想安装的 SDKs，或者点击 Wireless & RF 检查所有。单击 Next 并转到步骤10。
    
    ![2.3 p5](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-2.3p5.jpg)

8.  如果您单击 “Install by Device”，将出现安装设备支持对话框。在短时间的延迟后，它将显示你连接的设备。如果连接的设备没有显示，请单击 Refresh。下图显示连接设备显示之前和之后的安装设备支持对话框。

    ![2.3 p6](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-2.3p6.jpg)

9.  单击设备旁边的复选框来选择它。选择设备允许 Simple Studio 提供相关的软件包供您安装。单击 Next。

    ![2.3 p7](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-2.3p7.jpg)

10. 下一个对话框取决于您是否注册了套件包。如果你没有登录，您将没有访问限制内容，必须先登录（见下图 a）。如果你已经登录了但没有注册套件，你可以看到一些受限的内容而不是 Silicon Labs Thread（见下图 b）。点击 Register Kit。

    ![2.3 p8](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-2.3p8.jpg)

11. 如果您已经登录并注册了套件包，且看到 Thread 访问权限被授予，请单击 Next。

    ![2.3 p9](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-2.3p9.jpg)

12. Installation Options 对话框显示可以安装的工具和软件包。下面展示了选择 Silicon Labs Thread 产品组后和选择 EFR32MG 设备后的安装选项。在这两个视图中，您都可以取消勾选任何您不想安装的内容。如果您已通过产品组安装，则选择将根据您的需要进行过滤，而不是通过设备安装。如果您计划使用 GCC，请勾选 GNU ARM Toolchain。点击 Next \>。

    > 注意：以前的协议栈版本在 Other Options 中显示。

    ![2.3 p10](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-2.3p10.jpg)

13. Studio 显示一个审查许可对话框。接受显示的许可并单击 Finish。请注意，如果将来您安装了具有独立许可证的组件，则该对话框将再次出现。

    ![2.3 p11](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-2.3p11.jpg)

    安装需要几分钟。在安装过程中，Simple Studio 提供了查看和阅读选项，以了解更多关于环境的信息。

    ![2.3 p12](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-2.3p12.jpg)

14. 安装完成后，重启 Simple Studio。
15. 当 Simplicity Studio 重新启动时，你会被邀请去参观。要清除此选项，现在或在参观期间或之后的任何时间，单击 Exit tour。

    ![2.3 p13](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-2.3p13.jpg)

16. Launcher 透视图打开，但尚未完全填充。单击 devices 选项卡中的连接条目。请注意，USB 连接的 WSTK 设备被标识为 J-Link 设备，如图所示。

    ![2.3 p14](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-2.3p14.jpg)

17. 然后，Launcher 透视图将填充与您的硬件和协议栈相关的软件组件和功能。请注意，如果您看到 Stackless 应用程序，请单击链接以更改首选 SDK，如 [3.2 更改首选的 SDK](#32-更改首选的-sdkchanging-the-preferred-sdk) 中所述。 按 [3.3 更新适配器固件](#33-更新适配器固件updating-adapter-firmware) 中的说明更新设备固件。

    ![2.3 p15](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-2.3p15.jpg)

最后，如果您计划使用 IAR 作为编译器，请在文档列表中找到发行说明并检查软件版本要求，特别是对于 IAR-EWARM。 要安装 IAR-EWARM：
1. 在 Launcher 页面的 Resources 选项卡上，单击 Technical Support。
2. 向下滚动到页面底部，然后单击 Contact Support
3. 如果您尚未登录，请登录。
4. 单击 Software Releases 选项卡。在视图列表中选择 _Latest Thread Software。单击 Go。 在结果中是指向适当的 IAR-EWARM 版本的链接。
5. 下载 IAR 包（大约需要1小时）。
6. 安装 IAR。
7. 在 IAR 许可证向导中，单击 Register with IAR Systems to get an evaluation license。
8. 完成注册，IAR 将提供30天的评估许可。

------------------------------------------------------------------------------------------------------------------------

# **3. 启动程序透视图中的功能(Functionality in the Launcher Perspective)**

透视图由许多图块或窗格（称为视图）以及这些视图中的内容组成。您可以在透视图中执行许多功能，如下图所示。有关其中一些功能的更多信息将在本节后面提供。注意：您安装的版本可能与本节图形中显示的版本不同。

在工具栏（1）上，您可以：
* 登录或退出
* 打开应用程序设置
* 更新软件和固件（详情见 [3.1 下载更新或安装额外组件](#31-下载更新或安装额外组件downloading-updates-or-installing-additional-components) ）
* 打开工具菜单以访问工具，如 Simplicity Commander 和 Energy Profiler
* 在线搜索信息，包括社区论坛中的条目
* 改变视图（2）。当您打开 Simplicity IDE 或其他工具时，右上角会显示其视图按钮。使用这些按钮可以轻松导航回 Launcher 视图或其他视图。您可以通过展开或重定位视图，添加或删除视图来更改各种视图的布局。要返回默认布局，请右键单击右上角的视图按钮，然后选择 Reset。

在主视图中，您可以：
* 更改首选的 SDK（3，详情见 [3.2 更改首选的 SDK](#32-更改首选的-sdkchanging-the-preferred-sdk) - 遗留的功能，很少使用）
* 更改 debug 模式（4）
* 更新适配器固件（5，详情见 [3.3 更新适配器固件](#33-更新适配器固件updating-adapter-firmware) ）
* 创建多个部分的解决方案（6）。如果您针对涉及许多不同部分的复杂网络进行开发，则可以将它们全部添加到解决方案中，然后从列表中选择您正在处理的解决方案。您无需将硬件连接到计算机
* 从 Getting Started 和其他选项卡（7）中访问 demos、examples、documentation 和其他 resources。使用选择卡中的按钮控件可以管理这些选项组（分别是折叠所有、展开所有、自定义和显示所有）。详情见 [3.4 访问文档和其他资源](#34-访问文档和其他资源accessing-documentation-and-other-resources)

    ![3 p1](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-3p1.jpg)

## **3.1 下载更新或安装额外组件(Downloading Updates or Installing Additional Components)**

如果已安装组件的更新可用，则更新软件图标将为红色。如果 Simplicity Studio 检测到一个可用的更新，并且您在另一个透视图中，您将被通知一个更新可用。

要下载新的或更新组件，请单击更新软件图标。点击包管理器。注意：如果您是基于新设备进行安装，或者想要安装一个新产品组，您也可以通过这个对话框进行安装。在随后的对话框中，单击 Home 按钮返回到此对话框。注意，Studio 没有显示已经安装的 GNU ARM 工具链等选项。

![3.1 p1](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-3.1p1.jpg)

Simple Studio 在 Package Manager 对话框中显示了可用的更新或 SDKs。您可以更新全部或选择个别更新。单击 Package Manager 对话框中的选项卡，可以看到其他可用于安装的组件。使用过滤器缩小长列表。

![3.1 p2](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-3.1p2.jpg)

## **3.2 更改首选的 SDK(Changing the Preferred SDK)**

这是一个遗留的功能。一般来说，大多数 Silicon Labs 协议栈用户将拥有一个可供他们使用的 SDK，即 Gecko SDK 套件。

![3.2 p1](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-3.2p1.jpg)

在该套件中，您可以安装多个协议。在任何给定实例中使用的协议可以通过您选择的示例控制，也可以通过 “New Project” 界面控制您选择的协议栈。通常，您应该通过 Simplicity Studio 更新管理器添加或删除协议栈。 如果您需要在正常安装过程之外安装协议栈或 Gecko SDK 套件，您将收到单独的说明。

## **3.3 更新适配器固件(Updating Adapter Firmware)**

最初，启动器透视图可能不会显示可用的本地适配器固件。单击 “Download” 下载任何更新。

![3.3 p1](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-3.3p1.jpg)

如果更新可用，单击 “Install” 安装固件。

![3.3 p2](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-3.3p2.jpg)

安装了当前更新后，将显示版本。如果另一个固件更新可用，Studio 会通知您。

## **3.4 访问文档和其他资源(Accessing Documentation and Other Resources)**

Getting Started 选项卡提供对演示，示例程序和协议栈相关文档的访问。要 显示/隐藏 特定类别，请单击自定义按钮。选择或取消类别，然后单击 “OK”。如果您安装了多个协议的 SDK，则按协议对类别进行分组，如下面（b）中的软件示例所示。

![3.4 p1](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-3.4p1.jpg)

Documentation 选项卡结合了有关协议栈和硬件的文档。单击任何文档上的星形图标可将它移动到我最喜欢的文档列表。

![3.4 p2](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-3.4p2.jpg)

Compatible Tools 选项卡是通过 Tools 下拉菜单访问可用工具的另一种方式。

![3.4 p3](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-3.4p3.jpg)

Resources 选项卡提供对支持，营销材料和 Silicon Labs 社区的访问。

![3.4 p4](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-3.4p4.jpg)

------------------------------------------------------------------------------------------------------------------------

# **4. 关于演示和示例(About Demos and Examples)**

因为从头开始开发应用程序是困难的，Silicon Labs Thread SDK 附带了许多内置演示和示例，涵盖了最常见的用例。演示是预先构建的应用程序映像，您可以立即运行。在构建应用程序映像之前，可以修改软件示例。与演示相同名称的软件示例提供演示功能。您看到的演示和示例由所选部件决定。如果您使用的是包含多个部件的自定义解决方案，请务必单击您正在使用的部件，以仅查看适用于该部件的那些部分。

Silicon Labs 建议您从示例开始并根据您的需要进行修改，而不是从空白项目开始。这些示例提供了协议栈所需的默认配置以及可以构建的基本应用程序结构。然而，如果您想从空白项目开始的话，您可以这样做。


## **4.1 演示(Demos)**

演示是预先构建的应用程序示例，可以直接下载到设备中。

Light（SoC）：简单的可调光灯，与 switch 应用程序配合使用，演示 Thread 网络中基本的 ZCL over IP 功能。可调光灯参考设计中提供了更完整的可调光灯示例。

Switch：简单的调光开关。它与 light 应用程序一起在 Thread 网络中演示基本的 ZCL over IP 功能。

要在您的设备上下载并运行演示，请下拉演示列表并单击演示。在下一个对话框的 Mode 下拉列表中，选择 Run。单击 Start。

![4.1 p1](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-4.1p1.jpg)

## **4.2 软件示例(Software Examples)**

> 注意：为 EFR32xG12 及更新的部件提供的示例包括 Silicon Labs Gecko Bootloader 示例。为所有兼容的 Simplicity Studio SDK 提供了示例。为 Gecko Bootloader 示例配置安全性时，必须使用 Simplicity Commander，而不是 Simplicity Studio IDE 界面。有关使用 Gecko Bootloader 的更多信息，请参阅 **UG266：Silicon Labs Gecko Bootloader User Guide**。

Silicon Labs Thread 软件示例如下。标有 \* 的示例在启动器页面上没有贴图，但可以在 New Project 对话框中访问。请务必查看有关任何平台限制的完整示例说明。

\*Border Router Management App：这是针对 Unix 主机的 Silicon Labs 边界路由器参考设计的一个组件，它演示了使用  Thread Group Commissioning App 进行设备 commissioning 以及在 Thread 网络和相邻 IP 网络之间的进行路由。

\*Capacitive Touch Sensing Switch：该 ZLC over IP 传感器应用提供 magnetic reed switch 和 magnetic reed switch 的 开/关 状态，是 Silicon Labs 边界路由器参考设计的一个组件。

Client：该 client 应用程序充当无线传感器网络中的数据传感器。它将信息报告给充当接收器的 server 节点。这两个 client 示例及相关的 server 示例展示了一个简单的，基于 CoAP 的专有消息传递模型，而不是使用 ZCL/IP 作为应用层。

Client（Sleepy）：该 client 应用程序充当无线传感器网络中的嗜睡数据传感器。它将信息报告给充当接收器的 server 节点。

\*Contact Sensor：这种 ZLC over IP 传感器应用程序提供 magnetic reed switch 和 device tamper detection 的 开/关 状态，是 Silicon Labs 边界路由器参考设计的一个组件。

\*Dimmable Light：这种 ZCL over IP 灯应用程序为调光功能提供 pwm 控制，是边界路由器参考设计的一个组件。

Light（SoC）：该 light 应用程序在 ZCL 网络中充当简单的可调光灯。它与 switch 示例应用程序一起使用，以演示 Thread 网络中基本的 ZCL over IP 功能。可调光灯参考设计中提供了更完整的可调光灯示例。

\*Light（Host）：该 light 应用程序在 ZCL 网络中充当简单的可调光灯。它与 switch 示例应用程序一起使用，以演示 Thread 网络中基本的 ZCL over IP 功能。可调光灯参考设计中提供了更完整的可调光灯示例。

NCP-SPI：该网络协处理器（NCP）应用程序支持通过 SPI 接口与主机应用程序进行通信。它可以按配置构建，也可以使用自定义扩展进行扩充，以进行初始化，主循环处理，事件 定义/处理 以及与主机的消息传递。

NCP UART（Software Flow Control）：该 NCP 应用程序可以按配置构建，也可以使用自定义扩展进行扩充，以进行初始化，主循环处理，事件 定义/处理 以及与主机的消息传递。请注意，为了利用 WSTK 板的软件流控制，必须使用扩展头（“EXP”）引脚，因为 WSTK 上的 USB-to-serial 接口不支持软件流控制。

NCP UART HW（Hardware Flow Control）：该网络协处理器（NCP）应用程序支持通过具有硬件流控制（RTS/CTS）的 UART 接口与主机应用程序进行通信。该 NCP 应用程序可以按配置构建，也可以使用自定义扩展进行扩充，以进行初始化，主循环处理，事件 定义/处理 以及与主机的消息传递。

\*Occupancy Sensor：这种 ZLC over IP 传感器应用程序提供占用感应，温度测量，相对湿度测量和照度测量。它预期与边界路由器参考设计一起使用。

\*OTA Server（Host）：该应用程序充当 ZCL 网络中的一个简单 OTA server。它适用于实现 OTA client 插件的任何设备（例如 light 或 switch 示例应用程序），以演示 Thread 网络中基本的 ZCL over IP 引导加载功能。

Sensor/Actuator：这种 ZCL over IP 传感器/执行器 节点应用程序提供 LED0 和蜂鸣器（仅限 EM35x-DEV）控制，并报告温度和 button 0 的状态。它预期与边界路由器参考设计一起使用。

Server（SoC）：该 server 应用程序充当无线传感器网络中的数据接收器。它从充当传感器的 client 节点收集信息。

\*Server（Host）：该 server 应用程序充当无线传感器网络中的数据接收器。它从充当传感器的 client 节点收集信息。

\*Smart Outlet：这种 ZCL over IP 传感器和执行器应用提供插座 开/关 控制，过流和过温关闭，以及功率，温度，相对湿度和照度测量。它预期与边界路由器参考设计一起使用。

Switch：该 switch 应用程序充当 ZCL 网络中的简单调光器开关。它与 light 示例应用程序一起使用，以演示 Thread 网络中基本的 ZCL over IP 功能。

Thread Test Application：这是一个非常简单的应用程序，可用于针对 Thread Test Harness 的 Silicon Labs Thread 协议栈互操作性测试。它的目的是提供一个示例应用程序，该应用程序可以针对这个工具进行测试。它启用了 Thread Test Harness CLI 插件以及使这些 CLI 命令正常运行所需的所有库和插件。

\*Thread Test Host Application：这是一个非常简单的应用程序，可用于针对 Thread Test Harness 的 Silicon Labs Thread 协议栈互操作性测试。它的目的是提供一个示例应用程序，该应用程序可以针对这个工具进行测试。它启用了 Thread Test Harness CLI 插件以及使这些 CLI 命令正常运行所需的所有库和插件。

## **4.3 从一个空白应用程序开始(Starting with a Blank Application)**

虽然 Silicon Labs 强烈建议从其中一个示例应用程序开始开发，但如果您想从空白应用程序开始，则可以。
1. 在启动器页面点击 New Project。
2. 点击 Silicon Labs Thread，并点击 Next。
3. 使用一个空白应用程序检查开始，并点击 Next。
4. 命名您的应用程序，并点击 Next。
5. （如果已安装 IAR 和 GCC）选择工具链。默认情况下，设置为 GCC，但您可以将工具链更改为 IAR。您也可以在项目打开后更改工具链。
6. 点击 Finish。Simplicity IDE 将打开，但未配置任何内容。

------------------------------------------------------------------------------------------------------------------------

# **5. 应用程序开发入门(Getting Started with Application Development)**

在这些说明中，您将编译并加载两个示例应用程序 Light 和 Switch，以创建一个简单的 Thread 网络。在应用程序加载并且 Client 已加入到网络后，您可以在控制台和网络分析仪中观察网络中的流量。

在 Simplicity Studio 中使用示例应用程序时，您将执行以下步骤：
1. 选择一个示例应用
2. 生成应用程序文件
3. 编译并将应用程序（and, the first time, a bootloader）刷新到无线板上
4. 与应用程序交互

以下各节将详细介绍这些步骤。这些程序用于带有 EFR32MG12 的 WSTK。注意：您的 SDK 版本可能晚于过程插图中显示的版本。

SDK version 2\.4 及更高版本包含对硬件外围设备配置和管理方式的许多更改。打开示例或生成代码时，您可能会看到以下对话框。始终单击 Yes。

![5 p1](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5p1.jpg)

## **5.1 选择一个示例应用(Selecting an Example Application)**

1. 在 Launcher 透视图中，单击示例应用程序，在本例中为 Switch。您的项目将基于此示例以及您在左侧的 Devices 或 Solutions 选项卡中选择的设备。
    
    ![5.1 p1](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5.1p1.jpg)

2. 系统会询问您是否要切换到 Simplicity IDE。单击 Yes。

    ![5.1 p2](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5.1p2.jpg)

3. Simplicity IDE 将在 AppBuilder 视图中打开新项目，并将焦点放在 General 选项卡上。

    > 注意：您现在右上角的 Launcher 按钮旁边有一个 Simplicity IDE 按钮。
    
    确保显示的工具链是您要使用的工具链。如果同时安装了 IAR 和 GCC，则 GCC 是默认值。请注意，如果要编译小于 512 kB 的部件的示例（例如 EFR32xG1），则必须使用 IAR，因为 GCC 和 IAR 之间的代码大小不同。

    ![5.1 p3](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5.1p3.jpg)

4. 要更改工具链，请单击 Edit Architecture。在结果对话框中，选择所需的工具链，然后单击 OK。

    ![5.1 p4](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5.1p4.jpg)

> 注意：您还可以通过单击 Launcher 透视图中的 New Project 或从 Simplicity IDE 透视图中的 Project 菜单中选择 New，然后完成一系列对话框来启动示例项目。此路径允许您更多地输入项目创建，包括更改项目名称和修改默认板和部件选择。它还提供对其他示例的访问。

## **5.2 生成应用程序源文件(Generating the Application Source Files)**

1. 在 Simplicity IDE 中，点击 Generate。

    ![5.2 p1](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5.2p1.jpg)

    如果收到以下覆盖警告，请单击 OK。

    ![5.2 p2](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5.2p2.jpg)

2. 生成完成后，将显示对话框报告结果。单击 OK。

    ![5.2 p3](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5.2p3.jpg)

    生成的文件显示在 Project Explorer 视图中。

    ![5.2 p4](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5.2p4.jpg)

## **5.3 编译和刷新应用程序(Compiling and Flashing the Application)**

### **5.3.1 关于 Bootloading(About Bootloading)**

由于此示例应用程序是使用一个 bootloader（在 HAL 选项卡下配置）构建的，因此您需要在首次运行应用程序之前加载 bootloader。

![5.3.1 p1](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5.3.1p1.jpg)

bootloader 是存储在预留闪存中的程序，其可以初始化设备，更新固件映像，并可能执行一些完整性检查。Silicon Labs 网络设备使用两种不同模式执行固件更新的 bootloader：standalone（也称为独立引导加载程序）和 application（也称为应用程序引导加载程序）。应用程序引导加载程序通过使用存储在内部或外部存储器中的更新映像重新编程闪存来执行固件映像更新。Silicon Labs 建议您始终随应用程序一起刷新引导加载程序映像，以便从一开始就适当分配闪存使用情况。 有关 bootloader 的更多信息，请参阅 **UG103\.6：Application Development Fundamentals: Bootloading**。

2017年3月，Silicon Labs 推出了 Gecko Bootloader，这是一个可通过 Simplicity Studio IDE 配置的代码库，用于生成可与各种 Silicon Labs 协议栈一起使用的 bootloader。Gecko Bootloader 与所有 EFR32xG 部件一起使用。

Gecko Bootloader 使用专门的固件更新映像格式，更新映像以扩展名 **\.gbl**（GBL 文件）结尾。构建应用程序时，将生成 **\.s37** 和 GBL 文件。GBL 文件的确切格式取决于您选择的硬件。

> 注意：使用 Gecko Bootloader 时，必须使用 Simplicity Commander 启用某些配置选项，例如安全功能。请参阅 **UG266：Silicon Labs Gecko Bootloader User’s Guide**。

### **5.3.2 构建和刷新文件(Building and Flashing Files)**

1. 生成项目文件后，单击顶部工具栏中的 Build 控件。如果 Build 控件未可用，请单击设备。您的示例应用程序将根据其构建配置进行编译。您可以随时在 Project Explorer 视图中更改构建配置，方法是右键单击项目并转到 Build Configurations \> Set Active。

    ![5.3.2 p1](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5.3.2p1.jpg)

    在构建期间，将在窗口中报告进度，该窗口可以在右下角后台运行。

    ![5.3.2 p2](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5.3.2p2.jpg)

    构建完成在 Build Console 中报告。

    ![5.3.2 p3](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5.3.2p3.jpg)

    构建应该没有错误地完成。如果发生任何错误，它们将在控制台中突出显示。联系技术支持以获取帮助。

    ![5.3.2 p4](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5.3.2p4.jpg)

    > 注意：如果您在 Windows 平台上使用 GCC，并且遇到有关缺少的包含文件的构建错误，则可能遇到了有关 GCC 和长路径名的已知问题。如果发生这种情况，您可以通过打开 Windows 注册表并将 "HKLM\SYSTEM\CurrentControlSet\Control\FileSystem" 注册表项值设置为 1 来解决此问题。如果进行此调整后仍然遇到有关 GCC 和示例应用程序构建的问题，请联系 Silicon Labs 技术支持。

2. 要加载应用程序和 bootloader 映像，请首先确保硬件显示在 Device 透视图中。展开无线板以显示部件号，因为您需要找到正确的 bootloader 文件。请注意，生成示例的文件夹显示在 General 选项卡上。

    ![5.3.2 p5](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5.3.2p5.jpg)

3. 右键单击设备，然后选择 Upload Application。将显示 Application Image Upload 对话框。您的列表可能与以下示例不同。

    ![5.3.2 p6](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5.3.2p6.jpg)

4. 浏览已编译应用程序的文件夹，然后选择 GBL 文件（有关详细信息，请参见 [5.3.1 关于 Bootloading](#531-关于-bootloadingabout-bootloading)）。

    如果使用 GCC 编译映像，文件会在 \<folder on General tab\>\\GNU ARM vn\.n\.n - Default\. 中。

    如果使用 IAR EWARM 编译映像，文件会在 \<folder on General tab\>\\IAR ARM - \<qualifier\>\. 中。

5. 如果尚未加载 bootloader，请检查 Bootloader image，然后浏览到包含与无线板的部件号对应的预构建 bootloader 映像的文件夹。映像位于平台下的 Simplicity Studio bootloader 文件夹中（例如：C:\\SiliconLabs\\SimplicityStudio\\v4\\developer\\sdks\\gecko\_sdk\_suite\\\<version\>\\platform\\bootloader\\）。浏览到 sample-apps 和 bootloader-storage-spiflash 文件夹。选择相应的无线板部件号的 **\.s37** 文件
，例如 “bootloader-storage-spiflash-efr32mg12p432f1024gl125.s37”。

6. 检查 Erase Chip，确保在上传新映像之前擦除主闪存块。新用户通常会经常检查这一点：
    * After uploading 选项是 Run（立即开始执行代码）和 Halt（等待事件，例如调试器连接或手动启动引导序列）。在刚开始开发期间，您通常会将此设置保留为 Run。
    * Flash 选项确定存储位置，和是 Internal 和 External SPI。将选项设置为 Internal。
    
    您完成的对话框应类似于以下内容：

    ![5.3.2 p7](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5.3.2p7.jpg)

7. 单击 OK。加载进度显示在右下方。当加载进度清除时，如果 LED1 正在缓慢打开和关闭，则您的应用程序已加载到 WSTK 上。

    ![5.3.2 p8](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5.3.2p8.jpg)

8. 您可以右键单击该设备，然后选择 Launch Console。在控制台窗口中，单击 Serial 1 选项卡，然后按回车键。您应该看到与项目名称对应的提示。请注意，设备旁边的图标现在为绿色，表示与控制台的串行连接。

    ![5.3.2 p9](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-5.3.2p9.jpg)

现在重复 Switch 示例的过程，方法是单击右上角的 Launcher，然后按照 [5.1 选择一个示例应用](#51-选择一个示例应用selecting-an-example-application) 中的步骤进行操作。与 Light 示例一样，成功启动后，Switch 示例将打开和关闭 LED1，您可以在连接的控制台的 Serial1 选项卡上的提示中看到项目的名称。

> 注意：在加载其他应用程序之前，必须断开与控制台的连接。右键单击该设备，然后选择 Disconnect。

------------------------------------------------------------------------------------------------------------------------

# **6. 创建网络(Creating a Network)**

下载了 light 和 switch 应用程序后，即可创建网络。

启动时，light 示例应用程序自动启动网络操作。如果节点第一次启动，它将基于固定的预配置参数加入网络。如果它先前已加入网络，它将使用存储在非易失性存储器中的网络参数简单地恢复网络操作。

switch 必须具有与灯的绑定才能使其按钮执行任何操作。可以使用 EZ Mode commissioning 创建绑定。两个设备都必须处于 EZ Mode。要进入 EZ Mode，请同时按下电路板上的两个按钮，或在控制台中输入 zcl ez-mode start。创建绑定后，switch 将使用绑定与 light 通信。

light 示例应用程序对来自 switch 的 On/Off 和 Level Control 消息作出反应。在 switch 板上，button 0 调暗或关闭 light 上的 LED0，button 1 则是亮起或打开。快速按下并释放 button 以发送 开/关 消息。按住 button 可发送 变亮/暗淡 的消息。请注意，调光功能在控制台中正确报告，但在 LED 中几乎察觉不到。

light 控制台报告收到的消息。

![6 p1](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-6p1.jpg)

switch 控制台显示来自 light 示例应用程序的反馈。

![6 p2](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-6p2.jpg)

light 和 switch 示例应用程序都包含 OTA Bootload Client 支持，这意味着它们通过 OTA Bootload Cluster 支持无线升级。light 和 switch 都将在网络上自动启动一次 OTA Bootload 操作。switch 搜索 OTA Bootload server，而 light 充当 OTA Bootload server 并响应发现查询。有关 ZCL/IP OTA 升级的更多信息，请参阅 **UG278：Zigbee Cluster Library over IP (ZCL/IP) User’s Guide**。

------------------------------------------------------------------------------------------------------------------------

# **7. 使用网络分析仪(Using the Network Analyzer)**

现在您的网络正常运行，您可以使用网络分析仪工具评估传输的数据。

1. 单击右上角的 Launcher 按钮，然后从 Tools 菜单中选择 Network Analyzer。网络分析仪将打开，控制台窗口仍显示数据。

    ![7 p1](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-7p1.jpg)

2. 确保将网络分析仪设置为正确的协议以解码。选择 File \> Preferences \> Network Analyzer \> Decoding \> Stack Versions，验证它是否设置为正确的协议栈版本，而不是自动检测。如果需要更改它，请单击正确的协议栈，单击 Apply,，然后单击 OK。

    ![7 p2](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-7p2.jpg)

3. 要确保数据包正确解码，请输入 Thread 网络的 Master Key。出于安全原因，无法在运行时从 Thread 协议栈中检索此 key，因此开发人员必须通过其他方式知道该 key。虽然部署到现场的 Thread 应用程序在启动网络时应使用随机生成的 Master Key，但这些 Switch 和 Light 示例应用程序使用可在 **switch-implementation.c** 或 **light-implementation.c** 文件中找到的硬编码 Master Key。如下所示：
    ```c
    static const EmberKeyData masterKey = {
    { 0x65, 0x6D, 0x62, 0x65, 0x72, 0x20, 0x45, 0x4D,
      0x32, 0x35, 0x30, 0x20, 0x63, 0x68, 0x69, 0x70, },
    };
    ```

    此 key 可能已经作为 "Sensor/Sink Network" key 预先加载到网络分析仪中，但您可以使用以下说明验证 key 是否存在，或者如果 key 尚未出现在可用 key 列表中，则输入 key：
    1. 选择 File \> Preferences \> Network Analyzer \> Decoding \> Security Keys。
    
        ![7 p3](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-7p3.jpg)
    
    2. 检查所需的 key 是否已在列表中。请注意，可以单击 “Key” 列标题，按键字段按字母顺序对列表进行排序。
    3. 如果 key 已存在，请继续执行下面的步骤 6 以确保 key 已启用。
    4. 点击 New。
    5. 命名新条目，并在其中输入 key 的十六进制字节，省略任何非空白分隔符字符以及任何 “0x” 前缀。
    6. 通过启用与该 key 位于同一行的 A（Active）复选框，确保该 key 处于活动状态以进行解码。
    7. 单击 Apply。
    8. 单击 OK 以离开。

4. 右键单击 Light 设备，然后选择 Start Capture。对 Switch 设备执行相同的操作。
5. 如果您所处的环境中有许多无线设备，则可能会出现非常嘈杂的网络分析仪环境，这反映在 event traffic 和 map 中。
    
    要过滤 map 视图：
    1. 点击 map。在工具栏上，单击 PAN ID 按钮：
    
        ![7 p4](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-7p4.jpg)

    2. 在 map 中查找包含 ISA3 适配器名称或 WSTK 名称或其下方的 J-Link 序列号的点，该序列号与 Switch 设备所连接的调试适配器的名称相匹配。这表示所讨论的点代表 Switch 设备。
    3. 右键单击 Switch 设备的代表点，然后选择 Show only this PAN。

    要过滤 transactions：
    1. 在 Events 框中，单击其中一个 MLE Advertise events（蓝色）。
    2. 在 Event Detail 中，向下滚动，直到看到 Destination PAN ID。
    3. 右键单击它，然后选择 Add to filter。
    4. 单击绿色过滤器表达式字段旁边的应用过滤器按钮。
        
        ![7 p5](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-7p5.jpg)

6. 您应该在 Transactions 窗口中看到实时捕获的无线消息：
    
    ![7 p6](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-7p6.jpg)

分析更复杂的网络时，可以拖动和重新定位 map 中显示的项目。通过右键单击设备，您还可以显示连接并添加标签。标签不仅在 map 中有用，而且在日志中也很有用。要标记完整日志，请单击 From beginning。

![7 p7](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-7p7.jpg)

------------------------------------------------------------------------------------------------------------------------

# **8. 下一步(Next Steps)**

探索配置示例应用程序以满足您的需求。许多软件配置都可以通过插件完成。选择一个插件以查看有关它及其配置选项的更多信息（如果有）。

![8 p1](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-8p1.jpg)

Simplicity Studio 提供 Hardware Configurator 工具，允许您轻松配置新外围设备或更改现有外围设备的属性。许多 HAL 插件都提供了Hardware Configurator 选项。

![8 p2](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-8p2.jpg)

注意：如果更改硬件配置选项，则更改将保存到临时文件中。要使更改包含在项目中，必须选中以下对话框中的 Overwrite 复选框，该对话框在文件生成结束时显示。

![8 p3](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-8p3.jpg)

您还可以通过双击与其他项目文件一起生成的 **\.hwconf** 文件，或单击 HAL 选项卡上的 Open Hardware Configurator 来打开 Hardware Configurator 工具。单击引脚图下的 DefaultMode Peripherals 以更改为外设视图。某些配置选项仅可通过 Hardware Configurator 工具使用。

![8 p4](../Pic/Getting%20Started%20with%20Silicon%20Labs%20Thread-8p4.jpg)

See AN1115: Configuring Peripherals for 32-Bit Devices in Simplicity Studio for more information about the Hardware Configurator both within the Simplicity IDE and as a separate tool.

------------------------------------------------------------------------------------------------------------------------
