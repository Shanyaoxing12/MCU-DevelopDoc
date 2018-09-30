# **UG103.4：Application Development Fundamentals：HAL** <!-- omit in toc -->

Silicon Labs HAL（硬件抽象层）是系统硬件和软件之间的程序代码，其为可在多个不同硬件平台上运行的应用程序提供一致的接口。HAL 专为使用 EmberZNet PRO，EmberZNet RF4CE 和 Silicon Labs Thread 的开发人员而设计。本文档包含 Mighty Gecko（EFR32MG）系列的新信息以及基于 EM3x 的 MCU 系列产品的更新。

Silicon Labs 的应用程序开发基础系列涵盖了项目经理，应用程序设计人员和开发人员在开始使用 Silicon Labs 芯片，EmberZNet PRO 或 Silicon Labs Bluetooth Smart 等网络栈以及相关开发工具的嵌入式网络解决方案之前应该了解的主题。这些文档可以作为任何需要介绍开发无线网络应用程序的人或者是 Silicon Labs 开发环境的新手的起点。

关键点：
* 提供关于 HAL 如何组织的信息，包括命名约定，API 文件和目录结构。
* 包括 HAL 功能的每个主要子部分的概述。
* 描述如何使 HAL 适应您的特定硬件和应用程序要求。

------------------------------------------------------------------------------------------------------------------------

- [**1. 引言**](#1-引言)
- [**2. HAL API 组织**](#2-hal-api-组织)
- [**3. 命名约定**](#3-命名约定)
- [**4. API 文件和目录结构**](#4-api-文件和目录结构)
    - [**4.1 ARM Cortex-M3 SoC 平台的 HAL 实现**](#41-arm-cortex-m3-soc-平台的-hal-实现)
- [**5. HAL API 描述**](#5-hal-api-描述)
    - [**5.1 通用微处理器函数**](#51-通用微处理器函数)
    - [**5.2 令牌访问和模拟 EEPROM**](#52-令牌访问和模拟-eeprom)
    - [**5.3 外设访问**](#53-外设访问)
    - [**5.4 系统定时器控制**](#54-系统定时器控制)
    - [**5.5 引导加载**](#55-引导加载)
    - [**5.6 HAL 实用程序**](#56-hal-实用程序)
    - [**5.7 调试通道**](#57-调试通道)
        - [**5.7.1 Virtual UART**](#571-virtual-uart)
        - [**5.7.2 包跟踪支持**](#572-包跟踪支持)
- [**6. 自定义 HAL**](#6-自定义-hal)
    - [**6.1 编译时配置**](#61-编译时配置)
        - [**6.1.1 必需的定义**](#611-必需的定义)
        - [**6.1.2 可选的定义**](#612-可选的定义)
    - [**6.2 自定义 PCB**](#62-自定义-pcb)
    - [**6.3 修改默认实现**](#63-修改默认实现)

------------------------------------------------------------------------------------------------------------------------

# **1. 引言**

硬件抽象层（HAL）是系统硬件和软件之间的程序代码，其为可在多个不同硬件平台上运行的应用程序提供一致的接口。要利用此功能，应用程序应通过 HAL 提供的 API 访问硬件，而不是直接访问。然后，当您迁移到新硬件时，您只需要更新 HAL。在某些情况下，由于硬件上的极端差异，HAL API 也可能略有变化以适应新硬件。在这些情况下，更新的有限范围使得使用 HAL 比不使用 HAL 更容易迁移应用程序。

本文的引言部分给使用 EmberZNet PRO，EmberZNet RF4CE 或 Silicon Labs Thread 的所有软件开发人员一个建议。需要修改 HAL 或将其移植到新硬件平台的开发人员需要阅读整个文档，以了解如何在满足网络栈要求的同时进行更改。

------------------------------------------------------------------------------------------------------------------------

# **2. HAL API 组织**

HAL API 由以下功能部分组织，这些部分在 [5. HAL API 描述](#5-hal-api-描述) 中描述：
* 通用微处理器函数(Common microcontroller functions)：用于控制 MCU 行为和配置的 API。
* 令牌访问(Token access)：EEPROM，模拟 EEPROM（SimEEPROM）和令牌抽象。有关令牌系统的详细讨论，请参阅 **UG103.7：Application Development Fundamentals：Tokens**。
* 外设访问(Peripheral access)：用于控制和访问系统外围设备的 API。
* 系统定时器控制(System timer control)：用于控制和访问系统定时器的 API。
* 引导加载(Bootloading)：文档 **UG103.6：Application Development Fundamentals：Bootloading** 中介绍了引导加载的使用。
* HAL 实用程序(HAL utilities)：可能依赖于硬件能力的通用 API（例如，可能利用硬件加速的 CRC 计算）。
* 调试通道(Debug Channel)：当与栈的 DEBUG 构建一起使用时，API 跟踪、调试 printfs、断言与崩溃信息，以及 Virtual UART 的支持。

------------------------------------------------------------------------------------------------------------------------

# **3. 命名约定**

HAL 函数命名具有以下前缀约定：
* **hal**：Sample 应用程序使用的 API。您可以根据需要删除或更改这些函数的实现。
* **halCommon**：栈使用的 API，也可以被应用程序调用。自定义 HAL 修改必须保持这些函数的功能。
* **halStack**：只有栈使用此 API。不应从任何应用程序中直接调用这些函数，因为这可能违反时序约束或导致重入问题。自定义 HAL 修改必须保持这些函数的功能。
* **halInternal**：HAL 内部的 API。这些函数不是直接从栈中调用的，也不应直接从任何应用程序中调用。它们仅从 **halStack** 或 **halCommon** 函数中调用。您可以修改这些函数，但要小心地保持任何相关的 **halStack** 或 **halCommon** 函数本身的功能。

------------------------------------------------------------------------------------------------------------------------

# **4. API 文件和目录结构**

HAL目录结构和文件被组织起来以便于独立修改编译器，MCU 和 PCB 配置。
* **\<hal\>/hal\.h**：此主包含文件包含所有其他相关的 HAL 包含文件，您应将其包含在使用 HAL 功能的任何源文件中。大多数程序不应包含较低级别的包含，而应包括此顶级 **hal.h**。
* **\<hal\>/ember-configuration\.c**：此文件定义编译时可配置栈变量的存储和函数实现的默认实现。您可以通过在编译时定义预处理器变量并在应用程序中实现该函数的自定义版本来自定义大部分的这些函数。（详情请参阅软件的 API 参考中的 **ember-configuration-defaults.h**）
* **\<hal\>/micro/generic**：此目录包含用于 POSIX 兼容的系统上的通用 MCU 的文件。默认编译器是 GCC。

## **4.1 ARM Cortex-M3 SoC 平台的 HAL 实现**

ARM Cortex-M3 片上系统（SoC）平台有一个 HAL 实现，如下所示：
* **\<hal\>/micro/cortexm3**：此目录包含 cortexm3 的 HAL 实现，cortexm3 是 EM3x 和 EFR32 平台使用的处理器核心。此目录中的函数是 cortexm3 特定的，但并非特定于特定微控制器系列或变体（请参阅下一条目）。
* **\<hal\>/micro/cortexm3/{mcu\_family}**：此目录实现了特定 MCU 系列的特有功能，如基于 EM3x 的 MCU 系列的 **\<hal\>/micro/cortexm3/em35x**，包括 EM357，EM3588 和 EM346 等变体；或者用于 EFR32MG 的 **\<hal\>/micro/cortexm3/efm32**。
* **\<hal\>/micro/cortexm3/bootloader**：此目录实现了基于 Cortex M3 平台上使用的片上 bootloader 的相关功能，以便于运行时 加载/更新 应用程序。（可以在 **\<hal\>/micro/cortexm3/{mcu\_family}/bootloader** 中找到更多 MCU 特定的文件）
* **\<hal\>/micro/cortexm3/{mcu\_family}/board**：此目录包含定义外设配置和其他 PCB 级设置的头文件，如初始化函数。这些在 HAL 实现中用于为不同的 PCB 提供正确的配置。

------------------------------------------------------------------------------------------------------------------------

# **5. HAL API 描述**

本节概述了 HAL 功能的每个主要子部分。

## **5.1 通用微处理器函数**

通用微处理器函数包括 **halInit()**，**halSleep()** 和 **halReboot()**。大多数应用程序只需要调用 **halInit()**，**halSleep()**（通常仅是 ZED）和 **halResetWatchdog()**。函数 **halInit()**，**halSleep()**，**halPowerUp()**，**halPowerDown()** 等调用板（board）头文件中定义的适当的函数来初始化或关闭任何板级外设。

## **5.2 令牌访问和模拟 EEPROM**

网络栈使用持久性存储以在断电或设备重启时维护制造和网络配置信息。该数据存储在令牌中。令牌由两部分组成：用于映射到物理位置的键，以及与该键关联的数据。使用这个基于键的系统可以隐藏数据在应用程序中的位置，从而支持不同的存储机制，并使用闪存损耗均衡算法来减少闪存使用量。

> Note：有关 Silicon Labs 令牌系统的更多信息，请参阅 **token.h** 文件和文档 **UG103.7：Application Development Fundamentals：Tokens**。

由于 EM3x 和 EFR32 处理技术不提供内部 EEPROM，因此实现了模拟 EEPROM（也称为 sim-eeprom 和 SimEE），以使用一部分内部闪存进行栈和应用程序令牌存储。使用模拟 EEPROM 存储非易失性数据的部件在保证写入寿命周期方面具有不同级别的闪存性能。（有关在寿命和温度变化方面的最低闪存耐久性的详细信息，请参阅特定的 MCU 参考手册）。最近，模拟 EEPROM 的第 2 版已经发布。对于第 1 版，ARM Cortex MCU 使用 4 kB 或 8 kB（默认）上层闪存来存储来模拟 EEPROM。对于第 2 版，模拟 EEPROM 需要 36 kB 的上层闪存。由于写入寿命周期有限制，模拟 EEPROM 实现了耗损均衡算法，该算法有效地扩展了单个令牌的写入寿命周期数。

模拟 EEPROM 设计成尽可能透明地在令牌模块下方运行。应用程序只需要实现一个回调并定期调用一个实用函数。此外，还提供了一个状态函数，为应用程序提供有关模拟 EEPROM 使用情况的两个基本统计信息。

闪存页面的擦除受应用程序的控制，因为擦除一个页面将阻止中断服务 21 ms。您可以为此目的使用 **halSimEepromCallback()** 函数 - 虽然必须执行擦除以保持适当的功能，但应用程序可以安排它以避免干扰任何其他关键时序事件。此函数在 **ember-configuration.c** 文件中实现了一个默认处理程序，它将立即擦除闪存。应用程序可以通过定义 **EMBER\_APPLICATION\_HAS\_CUSTOM\_SIM\_EEPROM\_CALLBACK** 来覆盖此行为。

状态函数也可用于提供有关模拟 EEPROM 使用情况的基本统计信息。有关模拟 EEPROM 的设计、使用和其他注意事项的深入讨论，请参考文档 **AN703：Using the Simulated EEPROM for the EM35x and Mighty Gecko (EFR32MG) SoC Platforms**。

## **5.3 外设访问**

网络栈需要访问某些片上外设；此外，应用程序可以使用其他片上或板载外设。默认的 HAL 为所有必需的外设和一些常用的外设提供了实现。Silicon Labs 建议开发人员在 HAL 框架中实现额外的外设控制，以便将来轻松移植和升级栈。

> Note：通过参阅 HAL API 参考的 “Sample APIs for Peripheral Access” 部分，可以找到特定版本栈提供的外设控制。每个 Silicon Labs 平台的 API 参考中都提供了单独的 HAL API 参考。

## **5.4 系统定时器控制**

网络栈使用系统定时器来控制秒级或毫秒级的低分辨率定时事件。高分辨率（微秒级）时序通过中断进行内部管理。Silicon Labs 鼓励开发人员尽可能使用系统定时器控件或事件控件；这有助于避免复制功能和不必要地使用稀缺的闪存空间。例如，您可以使用函数 **halCommonGetInt16uMillisecondTick()** 来检查先前存储的值与当前值，并实现毫秒级延迟。

## **5.5 引导加载**

HAL 接口中也抽象了引导加载功能。请参阅栈的 API 参考以及文档 **UG103.6：Application Development Fundamentals：Bootloading** 以获取有关 bootloader 的使用和实现的详细说明。

## **5.6 HAL 实用程序**

HAL 实用程序包括可能依赖于硬件功能的通用 API（例如，可以利用硬件加速的 CRC 计算）。HAL 实用程序默认提供了崩溃和看门狗诊断、随机数生成和 CRC 计算。

## **5.7 调试通道**

网络栈 HAL 实现了一个与 Simplicity Studio 通信的调试通道。调试通道为栈和客户应用程序提供了双向带外机制，以便将调试统计信息和信息发送到 Simplicity Studio 进行大规模的分析。它在栈的 Debug 构建中提供 API 跟踪和调试 printfs，以及与栈的 Normal 构建一起使用时的断言和崩溃信息和 Virtual UART 支持。

栈有三个构建级别：
* Full Debug 为栈 API 提供 API 跟踪以及其他调试能力。
* Normal 不提供 API 跟踪或 **emberDebugPrintf()** 支持，但提供其他所有内容。
* Debug Off 没有任何 SerialWire 接口，因此没有 Virtual UART，也没有除包跟踪接口（PTI）之外的网络分析仪事件跟踪。PTI 使用一个调试适配器，如 ISA3 或无线入门套件（WSTK），但不依赖于 SerialWire。在 ARM Cortex 平台上，除了开发环境级调试之外，串行线接口还用于调试通道。

> Note：由于附加特性，Debug 级别大于 Normal 级别。

### **5.7.1 Virtual UART**

一些网络栈支持 Virtual UART（VUART）功能，作为其核心栈库的一部分，或通过提供基本调试功能的单独库，例如 EmberZNet 的 ZCL Application Framework 中的 Debug Basic Library。VUART，也称为 “双向调试”，其允许在被用于调试输入和输出的调试通道（SerialWire 接口）上使用普通串行 API。当其启用时，Virtual UART 在软件中被指定为串行端口 0。VUART 功能仅适用于 SoC 平台，并在 **EMBER\_SERIAL0\_MODE** 设置为 **EMBER\_SERIAL\_FIFO** 或 **EMBER\_SERIAL\_BUFFER** 时启用。

> Note：Virtual UART 与通过 **com\_device.h** 在 EFR32 板级支持包（BSP）中提供的虚拟 COM 端口（称为 “VCOM”）不同。VCOM 通过 WSTK 的板载 TTL-to-USB 转换器将 USART0 的物理串行端口连接路由回来，以用作 USB 主机的通信端口。VCOM 和 VUART 彼此独立，可以单独或一起启用，或都不启用。

启用 VUART 支持后，发送到端口 0 的串行输出封装在调试通道协议中，并由调试适配器（ISA3 或 WSTK）通过 SWO 和 SWDIO 线双向发送。原始串行输出将由 Simplicity Studio 的设备控制台工具显示，并且还将显示在调试适配器端口 4900 上。类似地，发送到适配器端口 4900 的数据将封装在调试通道协议中并发送到节点。然后，还可以使用普通串行 API 读取原始输入数据。

VUART 为调试构建提供了一个额外的输出端口，否则其将无法使用。

VUART 的以下行为与普通串行 UART 不同：
* **emberSerialWaitSend()** 不等待数据完成发送
* **emberSerialGuaranteedPrintf()** 是没有保障的
* **EMBER\_SERIALn\_BLOCKING** 可能不会阻塞

根据处理器与其他栈功能的忙碌程度，可能会丢弃更多串行输出。

### **5.7.2 包跟踪支持**

网络栈支持与 Simplicity Studio 一起使用的包跟踪接口（PTI）。此功能允许 Simplicity Studio 查看网络中所有节点接收和发送的所有数据包，而不会干扰这些节点的操作。包跟踪接口与开发套件中提供的无线电模块配合使用，但也可以在自定义硬件设计上启用。

自定义节点必须具有数据包跟踪端口（基于 EFR32 的设计上的 Mini-Simplicity Connector 接口的一部分）才能使用包跟踪功能。除了适当的硬件连接（例如基于 EM3x 设计的 PTI\_FRAME/PTI\_DATA 或基于 EFR32 设计的 FRC\_DFRAME/FRC\_DATA）以使用包跟踪功能外，**BOARD\_HEADER** 还必须定义 **PACKET\_TRACE** 宏。您可以使用提供的板头文件中的设置（由 Simplicity Studio 的 IDE 生成，带有 **\_board.h** 后缀）作为模板。

无论为构建选择何种调试级别，PTI 都是可用的，因为此支持由硬件提供。禁用此接口要求将 PTI\_FRAME 和 PTI\_DATA 引脚分配给不同的 GPIO 功能。有关板头文件的更多信息，请参阅 [6.2 自定义 PCB](#62-自定义-pcb)。

------------------------------------------------------------------------------------------------------------------------

# **6. 自定义 HAL**

本节介绍如何使 Silicon Labs 提供的标准 HAL 适应您的特定硬件和应用要求。

## **6.1 编译时配置**

下面的预处理器定义用于配置网络栈 HAL。它们通常在项目文件中定义，但根据编译器配置，它们可以在任何全局预处理器位置中定义。

### **6.1.1 必需的定义**

以下预处理器定义必须被定义：
* **PLATFORM\_HEADER**：平台头文件的位置。例如，EM3588 使用 **hal/micro/cortexm3/compiler/iar.h**。
* **BOARD\_HEADER**：板头文件的位置。例如，EM3588 开发板使用 **hal/micro/cortexm3/em35x/board/dev0680etm.h**。自定义板应将此值更改为新的文件名。
* **PLATFORMNAME**（例如，**CORTEXM3**）。
* **PLATFORMNAME\_MICRONAME**（例如，**CORTEXM3\_EM3588**）。
* **PHY\_PHYNAME**（例如，**PHY\_EFR32**）。
* **BOARD\_BOARDNAME**（例如，**BOARD\_BRD4151A** 或 **BOARD\_DEV0680**）。
* **CONFIGURATION\_HEADER**：为 **ember-configuration.c** 提供额外的自定义配置选项。

### **6.1.2 可选的定义**

以下预处理器定义是可选的：
* **APPLICATION\_TOKEN\_HEADER**：使用自定义令牌定义时，此预处理器常量是自定义令牌定义文件的位置。
* **DISABLE\_WATCHDOG**：此预处理器定义可以在不编辑代码的情况下完全禁用看门狗。使用此定义要非常谨慎，并且仅在实用或测试应用程序中使用，因为看门狗对于强大的应用程序至关重要。

对于 EM3x：
* **EMBER\_SERIALn\_MODE** = **EMBER\_SERIAL\_FIFO** 或 **EMBER\_SERIAL\_BUFFER**（n 是对应的 UART 端口）。如果串行驱动程序未使用此 UART，请保留此未定义。请注意，Buffer 串行模式还为 UART 启用了 DMA 缓冲功能。
* **EMBER\_SERIALn\_TX\_QUEUE\_SIZE** = 发送队列的大小（以字节为单位）（n 是对应的 UART 端口）。如果为此 UART 端口定义了 **EMBER\_SERIALn\_MODE**，则必须定义此参数。在 FIFO 模式下，此定义的值指定队列大小（以字节为单位），最大为 16383。在 Buffer 模式下，定义将队列大小表示为包缓冲区的数量，每个包缓冲区为 **PACKET\_BUFFER\_SIZE** 个字节（截至本文撰写时为 32 个字节）。
* **EMBER\_SERIALn\_RX\_QUEUE\_SIZE** = 接收队列的大小（以字节为单位）（n 是对应的 UART 端口）。如果为此 UART 端口定义了 **EMBER\_SERIALn\_MODE**，则此必须定义。该值始终以字节为单位进行量化（即使在 Buffer 模式下），最高可达 16383。
* **EMBER\_SERIALn\_BLOCKING**（n 是对应的 UART 端口）。如果此串行端口使用阻塞式 IO，则必须定义此选项。

对于 EFR32：
* **COM\_USARTn\_ENABLE**（n 是对应的 UART 端口）。这样可以启用指定 UART 的以进行写入和读取。这通常在 **com\_device.h** 中定义。
* **COM\_n\_RX\_QUEUE\_SIZE** = 接收队列的大小（以字节为单位）（n 是对应的 UART 端口）。如果未定义，则默认值为 64 个字节。
* **COM\_n\_TX\_QUEUE\_SIZE** = 发送队列的大小（以字节为单位）（n 是对应的 UART 端口）。如果未定义，则默认值为 128 个字节。
* **COM\_USARTn\_HW\_FC** 或 **COM\_USARTn\_SW\_FC**（n 是对应的 UART 端口）。这分别地启用硬件或软件流控制。

## **6.2 自定义 PCB**

通过使用 Simplicity Studio IDE 基于现有板头文件的副本生成板头文件，然后编辑生成的文件以匹配自定义板的配置，可以轻松地为目标硬件创建自定义 GPIO 配置。板头文件包括 HAL 使用的所有外部外设引脚的布局定义，以及用于初始化、上电和断开这些外设的宏。通过编译时指定的 **BOARD\_HEADER** 预处理器定义来标识板头文件。IDE 生成的板头文件通常具有 **\_board.h** 后缀，但是栈释放的 **hal/micro/cortexm3/{mcu\_family}/board** 文件夹中提供了几个特定参考板的预制板头文件。

修改用于外设连接的端口名称和引脚号（对于 EFR32 可能是位置号），以适用于自定义板硬件。通常可以通过参考板的原理图轻松确定这些定义。

新文件完成后，更改此项目的预处理器定义 **BOARD\_HEADER** 以引用新文件名。

除引脚分配修改外，功能宏还在板头文件中定义，其用于初始化，上电和断开任何板特定的外设。这些宏是：
* **halInternalInitBoard**
* **halInternalPowerDownBoard**
* **halInternalPowerUpBoard**

在每个宏中，您可以调用适当的 helper halInternal API，或者，如果功能足够简单，则直接插入代码。

除了板头文件之外，某些修改可能还需要您更改其他源文件。可能需要的情况包括：
* 使用不同的外部中断或中断向量
* 跨多个物理 IO 端口的功能
* 更改用于功能的核心外设（例如，使用不同的定时器或 SPI 外设）

在这些情况下，请参阅 [6.3 修改默认实现](#63-修改默认实现)。

## **6.3 修改默认实现**

网络栈 HAL 的功能被分组为具有类似功能的源模块。这些模块 - 源文件 - 可以单独轻松替换，允许自定义实现其功能。下表总结了 HAL 源模块。

![Table 6.1. HAL Source Modules](../Pic/Application%20Development%20Fundamentals-HAL-T6.1.jpg)

在修改这些外设之前，请确保您熟悉命名约定和硬件数据表，并注意遵守被替换功能的原始契约。Silicon Labs 建议您在开始对这些功能进行任何自定义之前联系客户支持，以确定进行所需更改的最简单方法。

------------------------------------------------------------------------------------------------------------------------
