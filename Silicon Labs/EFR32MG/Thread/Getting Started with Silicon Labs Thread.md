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
- [**2. 设置开发环境(Setting Up Your Development Environment)**](#2-设置开发环境setting-up-your-development-environment)
    - [**2.1 连接无线入门套件(Connect the WSTK)**](#21-连接无线入门套件connect-the-wstk)
    - [**2.2 注册开发套件(Register your Development Kit)**](#22-注册开发套件register-your-development-kit)
    - [**2.3 安装 Simplicity Studio 和 Silicon Labs Thread(Install Simplicity Studio and the Silicon Labs Thread Stack)**](#23-安装-simplicity-studio-和-silicon-labs-threadinstall-simplicity-studio-and-the-silicon-labs-thread-stack)


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

您可以通过 Simplicity Studio 的 Resources 选项卡访问位于 https://www.silabs.com/support 的 Silicon Labs 支持入口，如 [3.4 访问文档和其他资源]() 中所述。如果您在开发期间有任何疑问，可以使用该入口联系客户支持。

## **1.3 文档(Documentation)**

可以通过 Simplicity Studio 访问协议栈文档，如 [3.4 访问文档和其他资源]() 中所述。Simplicity Studio 还提供了硬件文档和其他应用笔记的链接。有关 Silicon Labs Thread 软件的更多详细信息，请参阅发行说明。

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

17. 然后，Launcher 透视图将填充与您的硬件和协议栈相关的软件组件和功能。请注意，如果您看到 Stackless 应用程序，请单击链接以更改首选 SDK，如 [3.2 更改首选 SDK]() 中所述。 按 [3.3 更新适配器固件]() 中的说明更新设备固件。

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
