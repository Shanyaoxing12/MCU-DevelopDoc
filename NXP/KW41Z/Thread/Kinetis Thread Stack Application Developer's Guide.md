# **Kinetis Thread Stack Application Developer's Guide** <!-- omit in toc -->

- [**1. Introduction**](#1-introduction)
- [**2. Example Applications**](#2-example-applications)
- [**3. Application Architecture**](#3-application-architecture)
    - [**3.1 Overview**](#31-overview)
    - [**3.2 IAR Embedded Workbench application project structure**](#32-iar-embedded-workbench-application-project-structure)
    - [**3.3 IAR Embedded Workbench compile time configuration**](#33-iar-embedded-workbench-compile-time-configuration)
        - [**3.3.1 IAR Embedded Workbench project options configuration**](#331-iar-embedded-workbench-project-options-configuration)
        - [**3.3.2 IAR Embedded Workbench header file configuration**](#332-iar-embedded-workbench-header-file-configuration)
    - [**3.4 MCUXpresso IDE application project structure**](#34-mcuxpresso-ide-application-project-structure)
    - [**3.5 MCUXpresso IDE compile time configuration**](#35-mcuxpresso-ide-compile-time-configuration)
        - [**3.5.1 MCUXpresso IDE project options configuration**](#351-mcuxpresso-ide-project-options-configuration)
    - [**3.6 Run time bootstrapping and initialization**](#36-run-time-bootstrapping-and-initialization)
    - [**3.7 Application task message queue and event handlers**](#37-application-task-message-queue-and-event-handlers)
    - [**3.8 Stack instance identifiers**](#38-stack-instance-identifiers)
- [**4. Thread Network APIs**](#4-thread-network-apis)
    - [**4.1 Factory default state**](#41-factory-default-state)
    - [**4.2 Scanning for networks**](#42-scanning-for-networks)
    - [**4.3 Joining a network**](#43-joining-a-network)
        - [**4.3.1 General network settings**](#431-general-network-settings)
            - [**4.3.1.1 Network scan channel mask**](#4311-network-scan-channel-mask)
            - [**4.3.1.2 Short PAN identifier (16-bit PAN ID)**](#4312-short-pan-identifier-16-bit-pan-id)
            - [**4.3.1.3 Extended PAN identifier (64-bit XPAN ID)**](#4313-extended-pan-identifier-64-bit-xpan-id)
            - [**4.3.1.4 Network name**](#4314-network-name)
        - [**4.3.2 Registering the network join event set handler**](#432-registering-the-network-join-event-set-handler)
        - [**4.3.3 Initiating joining**](#433-initiating-joining)
        - [**4.3.4 Joining a network with in-band commissioning**](#434-joining-a-network-with-in-band-commissioning)
            - [**4.3.4.1 Setting in-band configuration**](#4341-setting-in-band-configuration)
            - [**4.3.4.2 The IEEE EUI64 address**](#4342-the-ieee-eui64-address)
            - [**4.3.4.3 The PSKd device password**](#4343-the-pskd-device-password)
            - [**4.3.4.4 In-band commissioning network selection callback**](#4344-in-band-commissioning-network-selection-callback)
        - [**4.3.5 Joining a network with out-of-band commissioning**](#435-joining-a-network-with-out-of-band-commissioning)
            - [**4.3.5.1 Setting out-of-band configuration**](#4351-setting-out-of-band-configuration)
            - [**4.3.5.2 Thread master key**](#4352-thread-master-key)
            - [**4.3.5.3 Out-of-band commissioning network selection callback**](#4353-out-of-band-commissioning-network-selection-callback)
    - [**4.4 Creating a new Thread network**](#44-creating-a-new-thread-network)
    - [**4.5 Starting and configuring the commissioner module**](#45-starting-and-configuring-the-commissioner-module)
- [**5. Reading IP Address Configuration**](#5-reading-ip-address-configuration)
- [**6. Constrained Application Protocol (CoAP)**](#6-constrained-application-protocol-coap)
    - [**6.1 CoAP observe**](#61-coap-observe)
        - [**6.1.1 Enabling CoAP observe**](#611-enabling-coap-observe)
        - [**6.1.2 CoAP observe demo**](#612-coap-observe-demo)
        - [**6.1.3 APIs for CoAP observe**](#613-apis-for-coap-observe)
            - [**6.1.3.1 CoAP observe server**](#6131-coap-observe-server)
            - [**6.1.3.2 CoAP observe client**](#6132-coap-observe-client)
- [**7. Socket Data APIs**](#7-socket-data-apis)
- [**8. Low Power End Device Provisioning**](#8-low-power-end-device-provisioning)
- [**9. Revision History**](#9-revision-history)
- [**10. Copyright**](#10-copyright)


# **1. Introduction**

本文档说明如何开始开发一个Thread IPv6 mesh无线协议应用程序。

该文档还描述了应用程序执行的stack的配置和初始化，以及对特定用例的API调用的介绍。

下面是与执行stack操作相关的演示应用程序模块的详细描述。
* Scanning for networks
* Joining initiation
* Commissioning parameters
* Selecting a Parent
* Inspecting IP address configuration
* Access the IP data plane via Sockets and CoAP sessions
* Low-power-specific configuration

------------------------------------------------------------------------------------------------------------------------

# **2. Example Applications**

Thread stack与IAR® Embedded Workbench和MCUXpresso example applications一起提供，它们是应用程序开发者的起点。示例应用程序是Thread设备的主要类别模板：
* Router Eligible Device – 一种独立的Thread无线节点，其具有mesh路由能力，还可以充当Parent、转发、缓冲数据和代表附着在其上的child End Devices完成网络管理功能
* End Device – 一种独立的Thread无线节点，其无线收发器总是打开，但是可能没有板载资源来缓冲和转发 wireless mesh 中的routed data，或者充当Parent
* Low Power End Device – 一个类似End Device的应用模板，其主要的特点是：设备预配置为具有低功耗状态的微控制器，并且在设备生命周期的大部分时间内关闭无线电；该设备在几秒钟的间隔内周期“唤醒”，并主动轮询其Parent Router发送给它的数据，或者可以选择通过Parent Router向网络发送数据
* Border Router – 在部署时，启用IP stack以在Thread IEEE®802.15.4/6LoWPAN wireless mesh接口和auxiliary接口之间通过Thread network border转发消息；因此，消息可以到达上游设施，例如标准家庭局域网（LAN）或因特网和云服务器
* Host Controlled Device – 一种实现扩展、全功能、具有stack的Router capable configuration的应用程序，并激活一个UART/SPI/USB外围设备，该外围设备承载Thread Host Control Interface(THCI)协议帧，以便由host应用程序处理器或工具驱动Thread stack微控制器

应用程序共享一个类似的高级工具链项目结构，通过起点、应用程序文件、stack和系统配置来区分。

有关示例应用程序的更多详情，请参阅 Kinetis Thread Stack Demo Applications User’s Guide(document KTSDAUG)。

------------------------------------------------------------------------------------------------------------------------

# **3. Application Architecture**

## **3.1 Overview**

本节介绍由Kinetis Thread Stack示例应用程序实现的常规应用程序结构和体系结构，并推荐其作为新应用程序的 模板/起点。

Router Eligible Device 项目仅用作基础示例，其他示例仅在提及其区分特征时使用。

建议应用程序开发者选择与其需要的应用程序更接近的示例模板。但是，在测试或部署任何设备时，需要具有路由能力的设备来引导新的网络实体，并让其他设备（包括独立end devices）加入网络。

## **3.2 IAR Embedded Workbench application project structure**

Router Eligible Device 的基础示例可以从 \boards\frdmkw41z\wireless_examples\thread\router_eligible_device\freertos\iar 中访问并加载到IAR Embedded Workbench version 7.80或更高版本中。使用与FRDM-KW41Z开发平台对应的frdmkw41z配置作为基础示例。在下游子文件夹中，为FreeRTOS OS提供项目。FreeRTOS OS项目被用作以下示例的基础。workspace例子是：

**\boards\frdmkw41z\wireless_examples\thread\router_eligible_device\freertos\iar\ router_eligible_device.eww**

将workspace文件启动到IAR EWARM工具中可以在窗口左侧的Workspace panel中显示项目和单个文件组及文件组织。

Thread应用程序workspace包含一个独立的IAR项目：
* router_eligible_device – 主应用程序项目，配置为构建时输出二进制 S-Record(*.srec)文件，该文件可以加载到设备固件上运行。

下图显示了主应用程序项目文件组结构的扩展以及每个组中文件的功能。

![Figure 1. IAR Application Workspace](../Pic/Kinetis%20Thread%20Stack%20Application%20Developer's%20Guide-Figure1.jpg)

## **3.3 IAR Embedded Workbench compile time configuration**

Thread stack应用程序的编译时配置通过一组IAR EWARM和C模块结构（主要是预处理器（宏）定义）进行管理。

### **3.3.1 IAR Embedded Workbench project options configuration**

配置的设置主要包含在IAR Project Options panel和 \middleware\wireless\nwk_ip_1.2.4\examples\common 以及 \middleware\wireless\nwk_ip_1.2.4\examples\\\<application>\config 项目中的文件组中包含的 *.h 头文件。

要检查IAR Project Options panel中设置的配置选项，请右键单击应用程序项目，如 Figure 1 所示。

下图显示了全局预处理器选项。这些包括：
* 将 \middleware\wireless\nwk_ip_1.2.4\examples\\\<application>\config 中的文件设置为 Preinclude 文件，每当调用模块编译时默认包含该文件
* 预处理器定义的符号，包含应用程序项目的系统和stack的主要全局设置

![Figure 2.](../Pic/Kinetis%20Thread%20Stack%20Application%20Developer's%20Guide-Figure2.jpg)

同样，链接器部分包含应用程序设置，这些应用程序设置在链接器创建二进制可执行文件时应用。这些包括：
* 链接器文件用于代码和数据的设备内存布局
* 影响OTAP或串行（FSCI）引导加载程序是否包含在固件存储器映射中的符号定义，如果保留nvm段，则允许在RAM存储器中放置中断向量表，允许使用内部闪存来存储一个磁盘映像，配置用于nvm的闪存扇区的数量，并启用首次运行时用于nvm的闪存扇区的擦除

![Figure 3. EWARM Project Options Configuration – linker settings](../Pic/Kinetis%20Thread%20Stack%20Application%20Developer's%20Guide-Figure3.jpg)

### **3.3.2 IAR Embedded Workbench header file configuration**

通过 \middleware\wireless\nwk_ip_1.2.4\examples\common 中的头文件和 \middleware\wireless\nwk_ip_1.2.4\examples\\\<application>\config 中 Preinclude 头文件管理stack和应用程序编译时配置的其余部分。 \middleware\wireless\nwk_ip_1.2.4\examples\common 中的头文件的设置是应用在所有应用程序项目的默认全局设置。如果应用程序必须修改一组特定的全局设置，则可以通过 \middleware\wireless\nwk_ip_1.2.4\examples\\\<application>\config 中的 Preinclude 头文件覆盖它们。Preinclude 文件具有更高的优先级，因为它是在为源模块调用编译器时首先处理的。下图显示了头文件设置优先的方式，并且可以为应用程序的特定目的进行覆盖：

![Figure 4. Compile time configuration setting header files structure and precedence](../Pic/Kinetis%20Thread%20Stack%20Application%20Developer's%20Guide-Figure4.jpg)

## **3.4 MCUXpresso IDE application project structure**

如 Kinetis Thread Stack Demo Applications User’s Guide 的第7部分所示，成功加载sdk后，可以访问并加载适用于Router Eligible Device的基础示例，并将其加载到MCUXpresso IDE version 10.1.1或更高版本中。

可以使用导入SDK example(s)… link，从Quickstart Panel加载项目。

在下游子文件夹中，为FreeRTOS OS提供项目。FreeRTOS OS项目被用作以下示例的基础。

![Figure 5. MCUXpresso IDE application project structure](../Pic/Kinetis%20Thread%20Stack%20Application%20Developer's%20Guide-Figure5.jpg)

将workspace文件启动到MCUXpresso IDE工具中，在窗口左侧的C/C++ Projects面板中显示项目和单个文件组和文件组织。

![Figure 6. MCUXpresso IDE projects view](../Pic/Kinetis%20Thread%20Stack%20Application%20Developer's%20Guide-Figure6.jpg)

Thread应用程序workspace包含一个独立的MCUXpresso项目。
* router_eligible_device – 主应用程序项目，配置为构建时输出二进制 S-Record(*.srec)文件，该文件可以加载到设备固件上运行。

下图显示了主应用程序项目文件组结构的扩展以及每个组中文件的功能。

![Figure 7. MCUXpresso IDE application workspace](../Pic/Kinetis%20Thread%20Stack%20Application%20Developer's%20Guide-Figure7.jpg)

## **3.5 MCUXpresso IDE compile time configuration**

Thread stack应用程序的编译时配置通过一组MCUXpresso和C模块结构（主要是预处理器（宏）定义）进行管理。

### **3.5.1 MCUXpresso IDE project options configuration**

配置设置主要包含在MCUXpresso Project Settings panel和 \middleware\wireless\nwk_ip_1.2.4\examples\common 和 \middleware\wireless\nwk_ip_1.2.4\examples\\\<application>\config 项目中的文件组中包含的 *.h 头文件。

要查看MCUXpresso Project Settings panel中设置的配置选项，请右键单击应用程序项目，然后选择“Properties”选项。

下图显示了全局预处理器选项。这些包括以下内容：
* 将 **\middleware\wireless\nwk_ip_1.2.4\examples\\\<application>\config\config.h** 处的文件设置为Preinclude文件，每当调用源模块编译时默认包含该文件
* 预处理器定义的符号，包含应用程序项目的系统和stack的主要全局设置

![Figure 8. MCUXpresso IDE project options configuration – C compiler preprocessor settings](../Pic/Kinetis%20Thread%20Stack%20Application%20Developer's%20Guide-Figure8.jpg)

与IAR相比，当前的MCUXpresso版本不支持预处理链接器文件。创建二进制可执行文件时应用的应用程序设置包含在链接程序文件中，该文件可从托管链接程序脚本部分找到。这些包括以下内容：
* 影响RTOS堆内存大小的符号定义以及固件存储器映射中是否包含OTAP或串行（FSCI）引导加载程序。

## **3.6 Run time bootstrapping and initialization**

一旦Thread示例应用程序运行，FreeRTOS OS会引导系统并最终运行 **\middleware\wireless\nwk_ip_1.2.4\examples\common\app_init.c** 中定义的默认 main_task 函数。该函数表示默认的FreeRTOS应用程序级（application-level）任务。该函数通过调用其特定的初始化函数来初始化所有其他系统和IP stack模块。注意，main_task 对Thread和IP stack初始化函数 THR_Init 的调用初始化了core Thread stack模块并创建了一个新的FreeRTOS OS任务，该任务运行stack功能，状态机和消息处理。在Thread初始化时，将读取唯一的设备ID，默认情况下，该ID作为Thread接口上使用的 factory EUI64 的后缀。函数 THR_Init 在模块 **\middleware\wireless\nwk_ip_1.2.4\examples\common\app_thread_init.c** 中定义。

作为其初始化的一部分，其他系统模块（例如IEEE 802.15.4 MACPHY链路层或处理串行接口通信（UART，USB，SPI和IIC）通信的阻塞式串行管理器）创建新的FreeRTOS OS任务，以允许非阻塞异步处理。每个新的系统模块RTOS任务通过OS抽象模块 EventWait API（OSA_EventWait）输入一个 while(1) 无限循环等待事件发信号给任务。这是为了能够在系统运行时无限期地处理各个模块的事件和消息处理。

在完成所有初始化之后，main_task 进入自己的 while(1) 循环中，在该循环中它会重复调用 APP_Handler 以服务高级应用程序事件和功能，并允许在空闲时进入低功耗状态（具备低功耗配置），执行挂起操作的非易失性存储器模块和重置看门狗。

## **3.7 Application task message queue and event handlers**

必须在主应用程序文件中定义 APP_Handler（在基础示例中： **\middleware\wireless\nwk_ip_1.2.4\examples\router_eligible_device\src\router_eligible_device_app.c**）。它在默认应用程序任务上运行，如 main_task 循环中所调用。它的作用是通过应用程序消息队列 appThreadMsgQueue 提取从其他模块（如 core Thread）发送到应用程序的任何消息。对 NWKU_MsgHandler 的调用将一条消息取消并调用函数中指定的处理函数回调，从而确保在创建消息时请求的 RTOS 任务的上下文中完成处理。

为了处理消息循环中的事件，应用程序使用文件 **middleware\wireless\nwk_ip_1.2.4\base\stack_globals\event_manager_globals.c** 中的 StaticEventsTable 注册回调函数。

```c
gThrEvSet_NwkJoin_c, Stack_to_APP_Handler, &mpAppThreadMsgQueue, gThrDefaultInstanceId_c, TRUE
```

来自运行其较高优先级任务中的功能的其他模块的多个传入消息将保存在 appThreadMsgQueue 中，直到应用程序任务运行 APP_Handler，该 APP_Handler 将它们出列并调用与消息中的事件集标识符匹配的已注册处理程序。

这为利用RTOS提供的任务系统异步消息传递和处理提供了高度的灵活性。

## **3.8 Stack instance identifiers**

多个 Thread 和 IP stack API 允许通过 API 原型来寻址不同的运行时实体。在默认配置和大多数用例中，当调用需要 instanceId_t thrInstanceID 参数的 API 时，必须使用值 0（第一个默认 stack 实例的索引）作为参数。

# **4. Thread Network APIs**

## **4.1 Factory default state**

在其固件部署到设备后，应用程序的首次运行将进入 factory default idle state，即未连接到任何网络。对于部署在标准开发板上的示例，通过所有应用程序可用的板上 LED 灯闪烁蓝光来指示。

从 Thread network 的角度来看，当初始或复位到 Factory Default state 时，节点无线电关闭，设备关闭任何 Thread network。

为了将处于 Factory Default state 的设备带入到活动的 Thread network 中，应用程序可以选择以下的选项之一：

* 如果应用程序已在编译时被配置成包含 routing capability，则应用程序可以选择创建一个新的 Thread network 实体，并且当前节点可以充当其初始 Leader router。为了创建一个新的 Thread network，应用程序必须调用 THR_NwkCreate。该 API 以及其它的 network state commands 在 API 接口头文件 **\middleware\wireless\nwk_ip_1.2.4\core\interface\thread\thread_network.h** 中定义。应用程序也可以选择建立配置，以在编译时或动态地设置网络。
* 否则，应用程序可以选择使用 THR_Join 来扫描由其它节点创建的网络以加入。如果加入网络而不是创建网络，则应用程序可以设置某些参数，包括网络安全证书是否通过 DTLS in-band commissioning 获取，或者是否静态配置以测试或 out-of-band 获取。THR_Join 根据 iscommissioned 属性的值来触发 commissioning 或 attached。它可以设置为 0（pre-commissioned, commissioned out-of-band, the node only attaches to a scanned network）或 1（not pre-commissioned, the node perform in-band commissioning using DTLS）。

在加入之前，应用程序还可以选择通过 THR_Set 动态设置参数，并使用 THR_NwkScan 对网络进行单独的扫描。

## **4.2 Scanning for networks**

为了扫描范围内的网络，应用程序调用 THR_NwkScan，如下所示。

首先，为来自 stack 的 scan events 设置一个 handler：

```c
int32_t mThrInstanceId = 0;
    
static void APP_NwkScanHandler( void * param);

{gThrEvSet_NwkScan_c, APP_NwkScanHandler,&mpAppThreadMsgQueue, gThrDefaultInstanceId_c, TRUE },
from aStaticEventsTable
```

定义 handler：

```c
void APP_NwkScanHandler(void* param)
{
    thrEvmParams_t *pEventParams = (thrEvmParams_t *)param;
    thrNwkScanResults_t *pScanResults = &pEventParams->pEventData->nwkScanCnf;
        
    /* Handle the network scan result here */
    if(pScanResults)
    {
#if THREAD_USE_SHELL
        SHELL_NwkScanPrint(pScanResults);
#endif
        MEM_BufferFree(pScanResults);
    }
    /* Free Event Buffer */
    MEM_BufferFree(pEventParams);
}
```

声明 handler，启动扫描：

```c
thrNwkScan_t thrNwkScan;

thrNwkScan.maxThrNwkToDisc = 10; /* scan maximum 10 thread networks */
thrNwkScan.scanChannelsMask = 0x07FFF800; /* all channels*/
thrNwkScan.scanDuration = 2 /* exponential scale for duration as defined in IEEE 802.15.4 */;

thrNwkScan.scanType = gThrNwkScan_ActiveScan_c;

THR_NwkScan(mThrInstanceId, &thrNwkScan);
```

在 APP_NwkScanHandler 中接收扫描结果，其格式为 thrNwkScanResults_t* ，如下所示：

```c
typedef struct thrNwkScanResults_tag
{
    uint8_t *pEnergyDetectList thrNwkScan_t scanInfo;
    uint8_t numOfEnergyDetectEntries;
    uint8_t *pEnergyDetectList; /*!<One byte for each channel. Only the channels from
                                  scanInfo.scanChannelsMask should be handled;
                                  the rest of the channels are zeros */
    uint8_t numOfNwkScanEntries; /*!<Number of discovered network performing an active scan */
    thrNwkActiveScanEntry_t nwkScanList[]; /* where the network discovery list begins; Note that
                                              all these buffres shall be freed */
}thrNwkScanResults_t;
```

## **4.3 Joining a network**

应用程序调用 THR_Join 以发起并加入现有的 Thread network。该 API 支持启动 in-band commissioning 加入或 pre-commissioned settings matching an out-of-band commissioning configuration 加入。commissioning mode 可以在编译时使用 THR_DEV_IS_OUT_OF_BAND_CONFIGURD 参数进行设置。

### **4.3.1 General network settings**

下面描述的应用程序设置和设备属性通常会影响节点连接或创建网络。

#### **4.3.1.1 Network scan channel mask**

扫描信道掩码是一个 32 bitmap 值，其中 bits 11 到 26（LSB 到 MSB 索引）与 IEEE 802.15.4 允许的 2.4GHz 信道列表中的信道索引相匹配。

允许所有有效信道的全信道掩码格式为 0x07FFF800。应该将全信道掩码设置为产品设备的默认值：

```c
#define THR_SCANCHANNEL_MASK (0x07FFF800)
```

或者，允许信道掩码中单个信道值通过将 1 左移到相应的信道索引来设置。此设置可以在开发期间通过使用信道分离来将设备导向到网络来更好地进行加入受控测试，实例应用程序的默认值所选的信道为索引为 25 的信道：

```c
#define THR_SCANCHANNEL_MASK (0x00000001 << 25)
```

在运行时设置信道掩码：

```c
instanceId_t thrInstId = 0;
uint32_t aIndex = 0;

uint32_t scanMask = 0x07FFF800;
THR_SetAttr(thrInstId, gNwkAttrId_ScanChannelMask_c, aIndex, sizeof(uint32_t), &scanMask);
```

#### **4.3.1.2 Short PAN identifier (16-bit PAN ID)**

PAN（个域网）标识符的短格式是 IEEE 802.15.4 链路层所要求的 16-bit 值，以便允许节点在 PAN 内基于它们的 16-bit 短地址进行通信。因此，节点使用 4 个字节向彼此发送 messages：16-bit PAN ID 和 16-bit 短地址。在 Thread 中，网络层管理 PAN ID 短地址和节点短地址。在被视为一个 PAN 的同一个 Thread network 实体中，所有节点使用相同的 PAN ID，并且 messages 中不同的 PAN ID 指示不同的 Thread network。

建议应用程序不要更改 PAN ID 的默认设置（预设值为 0xFFFF），其指示初始 Leader 在创建网络时生成随机的 PAN ID，否则 Joiners 将 not filter 基于此字段的网络以加入。

```c
#define THR_PAN_ID 0xFFFF
```

以开发和测试作为目的，可以将 PAN ID 设置为与 0xFFFF 不同的特定值。如果 PAN ID 设置为特定值，初始 Leader 在创建网络时会明确地选择此 ID。因为具有不同的 PAN IDs，所以节点不加入或与现有网络交互。

```c
#define THR_PAN_ID 0xCAFE
```

在运行时设置 PAN ID（在创建或加入网络之前）：

```c
instanceId_t thrInstId = 0;
uint32_t aIndex = 0;

uint16_t panId = 0xCAFE;
THR_SetAttr(thrInstId, gNwkAttrId_PanId_c, aIndex, sizeof(uint16_t), panId);
```

#### **4.3.1.3 Extended PAN identifier (64-bit XPAN ID)**

PAN 扩展标识符（XPAN ID）是一个 64-bit 值，用于在多个 Thread network 实体间进行唯一标识和识别。其通过初始 Leader 随机生成，64-bit 值比 16-bit 的链路层 PAN ID 具有更低的重复冲突可能性。

建议应用程序不要更改 XPAN ID 的默认设置（预设值为 8 个 0xFF），其指示初始 Leader 在创建网络时生成随机的 XPAN ID，否则 Joiners 将 not filter 基于此字段的网络以加入。

```c
#define THR_EXTENDED_PAN_ID {0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF}
```

XPAN ID 通过 beacon transmissions 或 discovery messages 通告给 Joiner。

在运行时设置 XPAN ID（在创建或加入网络之前）：

```c
instanceId_t thrInstId = 0;
uint32_t aIndex = 0;

uint64_t xPanId = {0x00, 0xAA, 0xBB, 0xCC, 0xDD, 0xEE, 0xFF};
THR_SetAttr(thrInstId, gNwkAttrId_ExtendedPanId_c, aIndex, sizeof(uint64_t), &xPanId);
```

#### **4.3.1.4 Network name**

网络名称是可由应用程序设置为默认值的值，但建议通过具有丰富用户界面的允许创建网络的应用程序以请求网络用户输入网络名称的值。在这一点上，Thread 的网络名称是一个类似于 Wi-Fi 网络的 SSID 的标识符，允许在扫描或选择要加入的网络时更容易地表示网络。

示例应用程序将网络名称默认设置为 Kinetis_Thread。

```c
#define THR_NETWORK_NAME {14, "Kinetis_Thread"}
```

在运行时设置网络名称（例如，在网络创建之前，通过用户指示）。

```c
instanceId_t thrInstId = 0;
uint32_t aIndex = 0;

thrOctet16_t newName = {15, “MyThreadNetwork”};
THR_SetAttr(thrInstId, gNwkAttrId_NwkName_c, aIndex, sizeof(newName), &newName);
```

网络名称的长度限制为 16 个字节。

### **4.3.2 Registering the network join event set handler**

要监控加入过程期间由 stack 返回的 asynchronous messages，应用程序应为 network join event 集注册一个回调。下面的代码说明了示例程序如何注册回调 Stack_to_APP_Handler 以接收 joining events 和 messages：

```c
#define gThrDefaultInstanceId_c 0
static taskMsgQueue_t * mpAppThreadMsgQueue;

...

void APP_Init(void)
{
    /* Initialize pointer to application task message queue */
    mpAppThreadMsgQueue = &appThreadMsgQueue;

    /* Initialize main thread message queue */
    ListInit(&appThreadMsgQueue.msgQueue,APP_MSG_QUEUE_SIZE);
}

...

void Stack_to_APP_Handler(void* param)
{
    thrEvmParams_t *pEventParams = (thrEvmParams_t *)param;

    switch(pEventParams->code)
    {
        ...
        case gThrEv_NwkJoinCnf_Success_c:
        case gThrEv_NwkJoinCnf_Failed_c:
            APP_JoinEventsHandler(pEventParams->code);
            break;
        ...
        default:
            break;
    }

    /* Free event buffer */
    MEM_BufferFree(pEventParams->pEventData);
    MEM_BufferFree(pEventParams);
}

static void APP_JoinEventsHandler(thrEvCode_t evCode)
{
    if(evCode == gThrEv_NwkJoinCnf_Failed_c)
    {
    // JOINING HAS FAILED - could retry: (void)THR_NwkJoin(mThrInstanceId);
    ...
    }
    else if (evCode == gThrEv_NwkJoinCnf_Success_c)
    {
    // JOINING IS SUCCESFUL! – should indicate that to user
    ...
    }
}
```

### **4.3.3 Initiating joining**

要发起网络加入，应用程序必须调用 THR_NwkJoin：

```c
instanceId_t thrInstId = 0;
thrJoinDiscoveryMethod_t discMethod = gUseThreadDiscovery_c;

(void)THR_NwkJoin(thrInstId, discMethod);
```

发起 in-band 或 out-of-band commissioning 加入取决于编译时标志 THR_DEV_IS_OUT_OF_BAND_CONFIGURD，及运行时 stack 属性 gNwkAttrId_IsDevCommissisoned_c 的值。

在所有情况中（无论是 in-band 还是 out-of-band commissioning），加入过程的结果都是通过上节中介绍的 Join events 的回调系统接收的。

应用程序可以在加入 state machine 期间影响和设置选项（例如，通过执行特定的回调来选择加入那个网络，参考文件 **\middleware\wireless\nwk_ip_1.2.4\examples\common\app_thread_callbacks.c** 展示的实现）。

### **4.3.4 Joining a network with in-band commissioning**

#### **4.3.4.1 Setting in-band configuration**

如果 THR_DEV_IS_OUT_OF_BAND_CONFIGURED 设置为 FALSE（默认），则使用 in-band commissioning，并且必须有一个活动的 commissioner 才能允许设备加入到网络。

或者，在运行时（设备处于 factory reset state）通过更新匹配属性，来将设备设置为使用 In-Band commissioning，如下所示：

```c
instanceId_t thrInstId = 0;
uint32_t aIndex = 0;

bool_t isDeviceCommissioned = TRUE;

THR_SetAttr(thrInstId, gNwkAttrId_IsDevCommissioned_c, aIndex, sizeof(bool_t),
            &isDeviceCommissioned);
```

此外，IEEE EUI64 地址和 PSKd 设备的设置或属性会影响 commissioning 网络加入。

#### **4.3.4.2 The IEEE EUI64 address**

factory IEEE EUI64 是每个设备的唯一地址。EUI64 应该作为具有 IEEE 定义的特定格式的通用唯一链路层（MAC）地址提供。在这个点上，设备制造商应该获取一个供应商 OUI 前缀和 EUI 集作为后缀以为每个 Thread 接口创建和提供通用唯一的 EUI64。

在 Thread 中，EUI64 未用在 message 头中以发送到其它节点，反而使用隐私的随机扩展地址作为 MAC 帧头。然而，EUI64 仍然作为一个标识符被 commissioner 使用，以便：

* 通过在 network beacons 中通告一个 filter，控制（引导）具有独立的 EUI64 设备来请求加入特定网络。
* 通过用户向 commissioner 指示预期的 joiners，发起用于网络身份认证的 DTLS 会话以唯一地标识一个或多个设备。

出于上述目的，EUI64 通常通过用户输入地址或通过 commissioner 智能设备应用程序读取 QR 码或 NFC 标签中的值来输入到 commissioner 中。

默认情况下，示例应用程序具有地址 0x146E0A0000000001 集：

```c
#define THR_EUI64_ADDR    0x146E0A0000000001
```

该地址可以在运行时（在加入前）设置，如下所示：

```c
instanceId_t thrInstId = 0;
uint32_t aIndex = 0;

uint64_t ieeeEui64 = 0x146E0A0000000001;
THR_SetAttr(thrInstId, gNwkAttrId_IeeeAddr_c, aIndex, sizeof(uint64_t), &ieeeEui64);
```

#### **4.3.4.3 The PSKd device password**

在 Thread 中，PSKd 是每个设备的唯一密码或 序列号/标识符。它在 DTLS EC-JPAKE based key-exchange 中被用作shared secret password，其发生在 Joiner 设备和 commissioner 之间。PSKd 通常是在产品标签上显示的字母数字字符串或 QR 码。PSKd 的主要作用是确保只有通过物理访问设备的合法用户才能发起对网络的 commissioning。作为 Joiner 和 Commissioner 之间每个设备唯一的 secret shared，PSKd 还确保了 EC-JPAKE key exchange 的加密种子。

与 EUI64 类似，PSKd 通常也通过用户输入字符串或通过 commissioner 智能设备应用程序读取 QR 码或 NFC 标签中的值来输入到 commissioner 中。

默认情况下，示例应用程序将 PSKd 设置为“THREAD”，如下所示。PSKd 必须以 base32-thread（0-9，A-Y，除开 I，O，Q，Z）编码。定义中的数值为 PSKd 字符串的长度。默认值仅用于开发。每个产品设备应始终配备有一个唯一的 PSKd：

```c
#define THR_PSK_D {6, "THREAD"}
```

为了在运行时将 PSKd 设置为不同的值，应用程序可以设置匹配属性。字符串的最小长度为 6 字节，最大长度为 32 字节。例如，设置字符串值为 A1B2C3。

```c
instanceId_t thrInstId = 0;
uint32_t aIndex = 0;

thrOctet32_t newPskd = {6, “A1B2C3”};
THR_SetAttr(0, gNwkAttrId_PSKd_c, aIndex, sizeof(newPSKd), &newPSKd);
```

#### **4.3.4.4 In-band commissioning network selection callback**

除了为 Joiner 设置特定属性之外，应用程序还可能需要粒度控制如何扫描现有网络中的父系路由器，以通过 Joiner 节点进行 in-band commissioning。

为此，Thread stack 提供一个回调函数，其必须由应用程序实现以在 in-band commissioning 网络扫描期间引导 stack。该回调为 APP_JoinerDiscoveryRespCb，其参考实现在文件 **\middleware\wireless\nwk_ip_1.2.4\examples\common\app_thread_callbacks.c** 中展示。

回调接收如下三种类型的 events。

* 扫描初始化：gThrDiscoveryStarted_c - 此时，应用程序可以执行来自先前扫描的 discovery table 的进一步清除，或分配内存来处理网络和路由器传入的指示。
* 来自候选路由器接收到的每个 Discovery Responses 包的指示：gThrDiscoveryRespRcv_c - Discovery Response 属性可以在 pDiscoveryRespInfo 参数中找到。
* 扫描完成：gThrDiscoveryStopped_c - 此时，应用程序可以处理了可能的 parent list，并调用 THR_NwkJoinWithCommissioning 期望应用程序提供来自 Discovery Responses 的 filter list 以加入。

APP_JoinerDiscoveryRespCb 的参考实现如下所示：

```c
void APP_JoinerDiscoveryRespCb(instanceId_t thrInstId,
                               thrDiscoveryEvent_t event,
                               uint8_t lqi,
                               thrDiscoveryRespInfo_t* pDiscoveryRespInfo,
                               meshcopDiscoveryRespTlvs_t *pDiscoveryRespTlvs)
{
    bool_t addToJoiningList = FALSE;

    if (event == gThrDiscoveryStarted_c)
    {
        const uint16_t maxSize = THR_MAX_NWK_JOINING_ENTRIES * sizeof(thrNwkJoiningEntry_t);
        gNbOfNwkJoiningEntries = 0;
        if(gpNwkJoiningList == NULL)
        {
            gpNwkJoiningList = MEM_BufferAlloc(maxSize);
        }
        if(gpNwkJoiningList)
        {
            /* Reset the Network Joining list */
            FLib_MemSet(gpNwkJoiningList, 0, maxSize);
        }
    }
    else if ((event == gThrDiscoveryRespRcv_c) && gpNwkJoiningList)
    {
        thrNwkJoiningEntry_t joinerEntry = {0};
        joinerEntry.steeringMatch = gMeshcopSteeringMatchNA_c;
        joinerEntry.channel = pDiscoveryRespInfo->channel;
        joinerEntry.panId = pDiscoveryRespInfo->panId;
        FLib_MemCpy(joinerEntry.euiAddr, pDiscoveryRespInfo->aEui, 8);
        FLib_MemCpy(joinerEntry.aXpanId, pDiscoveryRespTlvs->pXpanIdTlv->xPanId,
                    pDiscoveryRespTlvs->pXpanIdTlv->len);

        if(pDiscoveryRespTlvs->pCommissionerUdpPortTlv)
        {
            joinerEntry.commissionerUDPPort = ntohas(pDiscoveryRespTlvs->pCommissionerUdpPortTlv->aPort);
        }
        if(pDiscoveryRespTlvs->pJoinerUdpPortTlv)
        {
            joinerEntry.joinerUDPPort = ntohas(pDiscoveryRespTlvs->pJoinerUdpPortTlv->aPort);
        }
        
        /* Joiner case */
        if(gMeshcopCommissionerMode != gThrCommissionerModeNative_c)
        {
            if(THR_DISC_RSP_VER_GET(pDiscoveryRespTlvs->pDiscRespTlv->verFlags) == THR_PROTOCOL_VERSION)
            {
                if (THR_GetAttr_IsDevCommissioned(thrInstId) == TRUE)
                {
                    addToJoiningList = TRUE;
                }
                else
                {
                    joinerEntry.steeringMatch = MESHCOP_CheckSteeringData(pDiscoveryRespTlvs->pSteeringDataTlv);
                    if(joinerEntry.steeringMatch != gMeshcopSteeringMatchNone_c)
                    {
                        addToJoiningList = TRUE;
                    }
                }
            }
        }
        /* Native Commissioner case */
        else
        {
            /* Join as a Native Commissioner. Add filters here.*/
            if( (THR_DISC_RSP_VER_GET(pDiscoveryRespTlvs->pDiscRespTlv->verFlags) == THR_PROTOCOL_VERSION) 
                && (THR_DISC_RSP_N_GET(pDiscoveryRespTlvs->pDiscRespTlv->verFlags) == 1) )
            {
                addToJoiningList = TRUE;
            }
        }
        if(addToJoiningList)
        {
            addToJoiningList = APP_JoinerNwkListAdd(&joinerEntry, thrInstId);
        }
    }
    else if ((event == gThrDiscoveryStopped_c) && gpNwkJoiningList)
    {
        /* Start the joining process. The list will be freed by the function. */
        MESHCOP_NwkJoinWithCommissioning(thrInstId, gpNwkJoiningList, gNbOfNwkJoiningEntries);
        gpNwkJoiningList = NULL;
        gNbOfNwkJoiningEntries=0;
    }
}
```

### **4.3.5 Joining a network with out-of-band commissioning**

#### **4.3.5.1 Setting out-of-band configuration**

如果 THR_DEV_IS_OUT_OF_BAND_CONFIGURED 设置为 TRUE，则会启用 out-of-band commissioning 和网络证书，主要是 Master Key 必须由应用程序提供。

或者，通过更新匹配属性，设备可以在运行时（设备处于factory reset state）使用 out-of-band commissioning，如下所示：

```c
instanceId_t thrInstId = 0;
uint32_t aIndex = 0;

bool_t isDeviceCommissioned = TRUE;

THR_SetAttr(thrInstId, gNwkAttrId_IsDevCommissioned_c, aIndex, sizeof(bool_t),
            &isDeviceCommissioned);
```

#### **4.3.5.2 Thread master key**

网络的 Thread Master Key 包含在基本密钥信息中，该信息由 Thread 指定的链路层和 MLE 层安全派生给网络中的所有节点。对于 in-band commissioning，Master Key 由 Joiner 节点通过 DTLS EC-JPAKE key exchange 获得。对于 out-of-band commissioning，应用程序必须提供密钥的实际值。

要在编译时将 master key 设置为特定值，需设置 THR_MASTER_KEY 预处理器定义：

```c
#define THR_MASTER_KEY {0x00, 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77, \
                        0x88, 0x99, 0xaa, 0xbb, 0xcc, 0xdd, 0xee, 0xff}

The key can be set at runtime (before joining) as shown below:
instanceId_t thrInstId = 0;
uint32_t aIndex = 0;

uint8_t masterKey[16] = {0x00, 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77,
                         0x88, 0x99, 0xaa, 0xbb, 0xcc, 0xdd, 0xee, 0xff};
THR_SetAttr(thrInstId, gNwkAttrId_NwkMasterKey_c, aIndex, sizeof(masterKey), masterKey);
```

#### **4.3.5.3 Out-of-band commissioning network selection callback**

如果 THR_DEV_IS_OUT_OF_BAND_CONFIGURED 设置为 TRUE，则设备将扫描配置的信道掩码中的 Thread networks，并根据 APP_OutOfBandSelectNwkWithBeaconCb 函数设置的应用回调行为来 attaches 网络。回调的默认实现可以在文件 **\middleware\wireless\nwk_ip_1.2.4\examples\common\app_thread_callbacks.c** 中找到。

此函数从扫描期间发现的现有网络中接收 beacon 帧：

```c
bool_t APP_OutOfBandSelectNwkWithBeaconCb(instanceId_t thrInstId, thrBeaconInfo_t *pThrBeacon)
{
    bool_t useThisNWK = FALSE;
    /* ADD YOUR FILTER HERE. */
    if(gaThrDeviceConfig[thrInstId].outOfBandChannel != 0)
    {
    /* If the channel is known, apply this filter */
        if(gaThrDeviceConfig[thrInstId].outOfBandChannel == pThrBeacon->channel)
        {
            useThisNWK = TRUE;
        }
    }
    else if(ntohall(gaThrDeviceConfig[thrInstId].xPanId) != THR_ALL_FFs64)
    {
        /* If the extended pan ID is known, apply this filter */
        if( FLib_MemCmp(pThrBeacon->payload.xpanId, gaThrDeviceConfig[thrInstId].xPanId,8) )
        {
            useThisNWK = TRUE;
        }
    }
#if ENABLE_MESHCOP_JOINER_FILTER_NWK_NAME
    else if(gaThrDeviceConfig[thrInstId].nwkName.length != 0)
    {
        /* If the network name is known, apply this filter */
        if(FLib_MemCmp(pThrBeacon->payload.nwkName, gaThrDeviceConfig[thrInstId].nwkName.aStr, 16))
        {
            useThisNWK = TRUE;
        }
    }
#endif
    else
    {
        useThisNWK = TRUE;
    }
    return useThisNWK;
}
```

## **4.4 Creating a new Thread network**

如 Router Eligible Device, Border Router 或 Host Controlled Device 文件所示，应用程序可以选择创建（引导，形成）新的 Thread network 实体。要创建一个新的 Thread network，应用程序必须调用：

```c
THR_NwkCreate(thrInstanceId);
```

使用默认 stack 配置时，THR_NwkCreate 的行为如下所示：

如果在 RF 信道掩码中指定了多个 RF 信道，则执行能量扫描以选择掩码中最适合创建网络的信道；这是通过 **\middleware\wireless\nwk_ip_1.2.4\examples\common\app_thread_callbacks.c** 中的回调 APP_NwkCreateCb 来处理的。在选择形成信道后，回调调用 THR_FormNwkOnChannel，创建一个新的 PAN ID 和 XPAN ID，并在各自的信道上启动一个 Leader Router。

THR_NwkCreate 调用的结果，返回一个 gThrEv_NwkCreateCnf_Success_c 或 gThrEv_NwkCreateCnf_Failed_c message events。

## **4.5 Starting and configuring the commissioner module**

Commissioner 是一个 Thread network 的网络管理模块和 IP 协议端点，用于允许新设备加入 Thread network 并且可以选择执行其它管理任务。Commissioner 模块不一定要部署到具有 Thread 接口的设备。Mesh Commissioning 协议允许家庭局域网（LAN）中的 commissioners 通过 Border Router 与 Thread networks 进行交互，以履行 commissioner 的角色。

Kinetis Thread Stack 提供一个完整的 Mesh Commissioning 协议 API，以允许设备启动和操作内部网络 Commissioner。

示例应用程序默认在 Thread 分区的 Leader 上启动一个 commissioner 模块与默认的安全配置，以允许所有具有 PSKd “THREAD” 的设备加入。这是通过在设备转换为 Leader 角色时调用 MESHCOP_StartCommissioner 来完成的，其可以在创建新的 Thread network 时发生，也可以在网络被分区并且 Leader 角色被另一个活动路由器取走时。

要在任何设备上激活 commissioner 模块，应用程序必须调用：

```c
MESHCOP_StartCommissioner(thrInstanceId);
```

Start Commissioner 的作用是 Thread 设备请求分区的 Leader 接受成为 Commissioner。如果 Leader 角色在同一设备上处于活动状态（Leader 和 Commissioner 角色被折叠在同一节点上），则该请求在本地自动接受，否则通过 Commissioner 和 Leader 之间的 Thread network 发送请求 message。

Leader 通过允许或拒绝请求来响应。在 Commissioner 设备上，这通过使用 gThrEv_MeshCop_CommissionerPetitionAccepted_c 和 gThrEv_MeshCop_CommissionerPetitionRejected_c event messages 向应用程序指示。

在用户将新设备加入网络之前，应该明确地配置 commissioner 以允许特定设备加入。设备的标识符是其 Thread 接口 IEEE EUI64 和设备密码（PSKd）。为了让 Commissioner 模块允许特定设备加入，必须将其 EUI64 和 PSKd 添加到 Commissioner 的 allowed joiner List 中：

要将新的 Joiner 添加到 allowed joiner list 中，应用程序必须调用：

```c
MESHCOP_AddExpectedJoiner(mThrInstanceId, aEui, aPSKd, sizeOfPSKdInBytes, TRUE)
```

可以使用头文件 **nwk_ip\core\interface\thread\thread_meshcop.h** 中定义的 MESHCOP_RemoveAllExpectedJoiners, MESHCOP_RemoveExpectedJoiner, MESHCOP_GetExpectedJoiner 来进一步操作由 Commissioner 模块存储的当前的 expected joiner list。

Expected Joiner list 的 APIs 仅在内部处理，并且在引导信息同步到活动路由器之前，不具有引导新设备加入的网络效应。为了传播和同步 expected joiner information 到 Thread network 的活动路由器（包括 Commissioner 的本地设备上的路由器实体），应用程序必须调用：

```c
MESHCOP_SyncSteeringData(mThrInstanceId, gMeshcopEuiMaskExpectedJoinerList_c)
```

也可以使用标志 gMeshcopEuiMaskAllFFs_c 来调用 MESHCOP_SyncSteeringData，以不 filter EUI64 和引导相应网络的所有设备，或通过 gMeshcopEuiMaskAllZeroes_c 拒绝所有设备（Commissioner “关闭”网络加入）。

一旦 commissioning 会话结束，在大多数情况下，强烈建议活动的 commissioner 应用程序显式关闭网络加入，通过如下调用：

```c
MESHCOP_SyncSteeringData(mThrInstanceId, gMeshcopEuiMaskAllZeroes_c)
```

要停用 commissioner，应用程序必须调用：

```c
MESHCOP_StopCommissioner(mThrInstanceId)
```

这将停止从 Commissioner 发送 keep alive messages 到 Leader，通过拒绝发送 keep alive 并释放 commissioner 占用的资源。

# **5. Reading IP Address Configuration**

在文件 **\middleware\wireless\nwk_ip_1.2.4\core\interface\thread\thread_utils.h** 中定义的 APIs 和在 **\middleware\wireless\nwk_ip_1.2.4\core\interface\thread\thread_types.h** 中的数据定义可以在节点成为 Thread network 的一部分后使用，以检查 Thread 节点的 Thread IPv6 地址配置。

Thread 定义的 IPv6 地址类型作为 nwkIPAddrType_t 定义的一部分，如下所示：

```c
typedef enum nwkIPAddrType_tag
{
    gLL64Addr_c = 0x00, /*!< Link-Local 64 address (the IID is MAC Extended address Which 
                        is not the factory-assigned IEEE EUI-64,)*/
    gMLEIDAddr_c = 0x01, /*!< Mesh-Local Endpoint Identifier address (the IID is randmon) */
    gRLOCAddr_c = 0x02, /*!< Routing Locator address (the IID encodes the Router and Child IDs.)*/
    gGUAAddr_c = 0x03, /*!< Global Unicast Address*/
    gAnycastAddr_c = 0x04, /*!< Anycast IPv6 addresses */
    gAnyIpv6_c = 0x05, /*!< All IPv6 address */
    gAllThreadNodes_c = 0x06, /*!< All Thread nodes address */
} nwkIPAddrType_t;
```

THR_NumOfIP6Addr 函数返回特定类型的 bound IPv6 地址的数量。

THR_GetIP6Addr 函数返回 IP 地址类型参数指定的 bound IPv6 地址。在 pDstIPAddr 位置应该分配足够的内存空间来复制所有请求的 IPv6 地址。应用程序应该使用 THR_NumOfIP6Addr 来查找需要多少空间来获取请求的 IPv6 地址。

为了检查 stack 使用的 IP 媒体接口的地址配置，应用程序还可以使用 **\middleware\wireless\nwk_ip_1.2.4\core\interface\modules\ip_if_management.h** 中的高级 APIs。

# **6. Constrained Application Protocol (CoAP)**

Kinetis Thread stack 提供一个高级 CoAP 模块，以在 UDP 上使用受限应用协议。CoAP 模块用于 stack 和应用程序。

下面的代码示例介绍了示例应用程序如何使用 CoAP。

初始化 CoAP URI 路径的回调配置结构：

```c
coapRegCbParams_t cbParams [] = { { APP_CoapCallback, (coapUriPath_t*)&gURI_OPTION } }
const coapUriPath_t gURI_OPTION = {SizeOfString("option"), "option"};
```

通过创建一个 CoAP 实例向 CoAP 模块注册服务：

```c
uint8_t mCoapInst;
coapStartUnsecParams_t coapParams = {COAP_DEFAULT_PORT, AF_INET6};
mCoapInst = COAP_CreateInstance(NULL, &coapParams, gIpIfSlp0_c, (coapRegCbParams_t*) cbParams, 1);
```

当要在实例上发送数据时，打开到远程目标地址的会话：

```c
coapSession_t *pSession = NULL;
uint8_t buffer[3] = { 0x01, 0x02, 0x03};
uint8_t * pCoapPayload = &buffer[0]; /* This variable MUST be set to a valid location */
uint8_t payloadSize = sizeof(buffer); /* This variable MUST be set to a valid size */
pSession = COAP_OpenSession(mCoapInst);
if (NULL != pSession)
{
    /* initialize to Non confirmable by default for multicast */
    coapMsgTypesAndCodes_t coapMessageType = gCoapMsgTypeNonPost_c;

    /* do not use the callback for non-confirmable */
    pSession->pCallback = NULL;

    /* set remote IP address */
    FLib_MemCpy(&pSession->remoteAddr, &remoteAddress, sizeof(ipAddr_t));

    /* add options to message */
    COAP_SetUriPath(pTxSession, (coapUriPath_t*)&gURI_OPTION);

    /* application can change to Confirmable option if not unicast and set an ACK callback */
    if (!IP6_IsMulticastAddr(&gCoapDestAddress))
    {
        coapMessageType = gCoapMsgTypeConPost_c;
        pSession->pCallback = CoapCallback;
    }

    /* Send the COAP frame */
    COAP_Send(pSession, coapMessageType, pCoapPayload, payloadSize);
}

An example of callback called for an option follows:
static void APP_CoapLedCb(coapSessionStatus_t sessionStatus,
                          void *pData,
                          coapSession_t *pSession,
                          uint32_t dataLen)
{
    /* Process the command only if it is a POST method */
    if((pData) && (sessionStatus == gCoapSuccess_c) && (pSession->code == gCoapPOST_c))
    {
        APP_ProcessLedCmd(pData, dataLen);
    }

    /* Send the reply if the status is Success or Duplicate */
    if((gCoapFailure_c != sessionStatus) && (gCoapConfirmable_c == pSession->msgType))
    {
        /* Send CoAP ACK */
        COAP_Send(pSession, gCoapMsgTypeAckSuccessChanged_c, NULL, 0);
    }
}
```

## **6.1 CoAP observe**

CoAP Observe 模块是 CoAP 核心协议的扩展。源文件位于 **\middleware\wireless\nwk_ip_1.2.4\examples\common\app_coap_observe.c** 。

测试应用程序随 Observe 模块一起提供。应用程序源文件位于 **\middleware\wireless\nwk_ip_1.2.4\examples\common\app_observe_demo.c** 。应用程序需要启用 Shell 模块。

CoAP Observe 工作在 Client 和 Server 模型上。Server 具有更改特定 resource 的 状态/值 的权限。Client 订阅 Server，以接收特定 resource 的更新。

### **6.1.1 Enabling CoAP observe**

所有应用程序项目默认禁用 CoAP Observe。其可以从以下的所有应用程序配置文件中启用：

**\middleware\wireless\nwk_ip_1.2.4\examples\\\<thread_application>\config\config.h**

```c
/* Enable CoAP Observe Client */
#define COAP_OBSERVE_CLIENT 1
/* Enable CoAP Observe Server */
#define COAP_OBSERVE_SERVER 1
```

CoAP Observe Client 和 Server 从 Shell 命令行启动。对于不支持 Shell 的项目，请调用 APP_ObserveStartClient()来初始化 CoAP Observe Client 和/或 调用 APP_ObserveStartServer()来初始化 Observe Server。有关接口函数的更多信息，请参阅下面的部分。

### **6.1.2 CoAP observe demo**

该 demo 演示如何将更改其状态的 resource 从 Server 报告给 Client。Server 拥有 resource “/number”的权限，“/number”即每 20 秒产生一次的随机数。每次更改 resource 后，Server 都会通知其订阅者。

Client 订阅 resource “/number”，并在 resource 的每个状态更改后收到 CoAP NON message。

在 Server 端，启动 CoAP Observe server：

```shell
$ coap start observe server
Success!
```

在 Client 端，启动 CoAP Observe Client,输入server 的 IPv6 地址和 resource。

```shell
$ coap start observe client fd01::3ead:4973:da79:49cd:521d /number

$ new value: 0xC01BFAB3 from fd01::3ead:4973:da79:49cd:521d
$ new value: 0xFAADB6CF from fd01::3ead:4973:da79:49cd:521d
$ new value: 0x1160E9EB from fd01::3ead:4973:da79:49cd:521d

$ coap stop observe client fd01::3ead:4973:da79:49cd:521d /number
$ new value: 0x5C261871 from fd01::3ead:4973:da79:49cd:521d
```

### **6.1.3 APIs for CoAP observe**

要创建一个应用程序以使用 CoAP Observe 功能，可以使用以下 API。


#### **6.1.3.1 CoAP observe server**

```c
uint8_t COAP_Server_InitObserve(ipIfUniqueId_t ipIfId, coapStartUnsecParams_t *pCoapStartUnsecParams, 
                                coapRegCbParams_t* pCallbacksStruct, uint32_t nbOfCallbacks)
```

该函数启动在给定 IP 接口上运行的 CoAP Observe server。它还创建 CoAP 实例并为具有特定 resource 的传入 messages 注册回调函数。

回调和 resource 定义如下。

```c
#define URI_OBSERVE_RESOURCE "/number"
const coapUriPath_t gURI_OBSERVE_RESOURCE = {SizeOfString(URI_OBSERVE_RESOURCE), URI_OBSERVE_RESOURCE};
#define COAP_OBSERVE_RESOURCE \
{\
{APP_ObserveServerRcvReqCb, (coapUriPath_t*)&gURI_OBSERVE_RESOURCE}\
}
```

server 应该保留一个序列号，每次 resource 更新时都必须递增序列号。

```c
void COAP_Server_NotifyObservers(coapUriPath_t* pResource, coapMessageTypes_t coapMsgType, 
                                 uint8_t sequenceId, uint8_t* pValue, uint8_t valueLen)
```

该函数通知用户列表中的所有 clients 指定的 resource。它以给定的有效载荷发送一个 CoAP message(CON/NON)，用 resource 的新值表示。

```c
bool_t COAP_Server_AddObserver(coapSession_t* pSession, coapUriPath_t* pResource)
```

将一个 observer 添加到 observers 列表中。

```c
bool_t COAP_Server_RemoveObserver(coapSession_t* pSession, coapUriPath_t* pResource)
```

从 observers 列表中移除一个 observer。

#### **6.1.3.2 CoAP observe client**

```c
uint8_t COAP_Client_InitObserve(ipIfUniqueId_t ipIfId, coapStartUnsecParams_t *pCoapStartUnsecParams,
                                coapRegCbParams_t* pCallbacksStruct, uint32_t nbOfCallbacks)
```

该函数启动在给定 IP 接口上运行的 CoAP Observe client。它还创建 CoAP 实例并为具有特定 resource 的传入 messages 注册回调函数。

```c
void COAP_Client_StartObserving(ipAddr_t* pServerIpAddr, coapUriPath_t* pResource, coapCallback_t rcvReplyCb)
```

发送 CoAP GET message 并要求订阅。

```c
void COAP_Client_StopObserving(ipAddr_t* pServerIpAddr, coapUriPath_t* pResource, coapCallback_t rcvReplyCb)
```

发送 CoAP GET message 并取消订阅。

# **7. Socket Data APIs**

Socket API 允许在 Thread IPv6 network 和 exterior network destinations（外部网络目标）上创建通用的 BSD/POSIX-style IP sockets。

以下代码块展示如何创建 socket 并通过 socket 将 UDP 数据发送到 IPv6 目标地址和端口：

```c
int32_t socketDescriptor = gBsdsSockInvalid_c;

socketDescriptor = socket(AF_INET6, SOCK_DGRAM, IPPROTO_UDP);

if (socketDescriptor != gBsdsSockInvalid_c)
{
    sockaddrIn6_t socketAddrInfo;

    ipAddr_t IPV6_REMOTE_ADDRESS = { 0x20, 0x01, 0x00, 0x00, 0x00, 0x00, 0xD0, 0x00, \
                                     0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00 };

    uint16_t remotePort = 1234;

    uint8_t dataToSend[] = { 0x41, 0x42, 0x43 }; // "ABC"

    uint8_t sendFlags = 0;

    socketAddrInfo.sin6_family = AF_INET6;
    IP_AddrCopy(&socketAddrInfo.sin6_addr, &REMOTE_ADDRESS);
    socketAddrInfo.sin6_port = remotePort;
    socketAddrInfo.sin6_scope_id = gIpIfSlp0_c;
    socketAddrInfo.sin6_flowinfo = BSDS_SET_TX_SEC_FLAGS(1, 5);

    sendto(socketDescriptor, dataToSend, sizeof(dataToSend), sendFlags, (sockaddrStorage_t *) &socketAddrInfo, sizeof(sockaddrStorage_t));
}
```

以下代码块展示如何创建 UDP socket 并将其 bind 到任何本地 IPv6 地址和端口：

```c
/* application must track serverSocketDescriptor and serverSocketAddrInfo while the socket is
needed and release them after shutdown */

int32_t serverSocketDescriptor;

sockaddrIn6_t serverSocketAddrInfo;

...

/* for initialization and socket binding */

serverSocketDescriptor = socket(AF_INET6, SOCK_DGRAM, IPPROTO_UDP);

if (serverSocketDescriptor != gBsdsSockInvalid_c)
{
    uint16_t localPort = 1234;
    
    serverSocketAddrInfo.sin6_family = AF_INET6;
    IP_AddrCopy(&serverSocketAddrInfo.sin6_addr, &in6addr_any);
    serverSocketAddrInfo.sin6_port = localPort;
    serverSocketAddrInfo.sin6_scope_id = gIpIfSlp0_c;
    serverSocketAddrInfo.sin6_flowinfo = BSDS_SET_TX_SEC_FLAGS(1, 5);

    bind(serverSocketDescriptor, (sockaddrStorage_t*) &serverSocketAddrInfo, sizeof(sockaddrStorage_t));

    Session_RegisterCb(serverSocketDescriptor, ServerSessionCallback, &appThreadMsgQueue);
}

...

/* Data session callback */

void ServerSessionCallback(void *pPacket)
{
    sessionPacket_t *pSessionPacket = (sessionPacket_t*) pPacket;

    sockaddrIn6_t *pClientAddrInfo = (sockaddrIn6_t*) (&pSessionPacket->remAddr);

    /* Process data in pSessionPacket->pData */
    ...

    /* Must free the packets after processing */
    MEM_BufferFree(pSessionPacket->pData);
    
    MEM_BufferFree(pSessionPacket);
}

...

/* To shut down */
shutdown(serverSocketDescriptor, 0);

Session_UnRegisterCb(serverSocketDescriptor);
```

使用 socket API 的更多示例包含在 **\middleware\wireless\nwk_ip_1.2.4\examples\common\app_socket_utils.h** 和 **.c** 文件中。

# **8. Low Power End Device Provisioning**

Low Power end devices 示例被设置为使用以下设置自动启用微控制器的低功耗模式（如 **\middleware\wireless\nwk_ip_1.2.4\examples\low_power_end_device\config\config.h** 所示）。

```c
#define gLpmIncluded_d 1
#define cPWR_UsePowerDownMode 1
```

请参阅 **\middleware\wireless\framework_5.3.4\LowPower\Interface\\\<PLATFORM>** 上的配置文件。

例如，

**\middleware\wireless\nwk_ip_1.2.4\examples\common\app_framework_config.h** 用于配置关机模式。demo 示例使用宏定义设置的模式 3：cPWR_DeepSleepMode。

# **9. Revision History**

This table summarizes revisions to this document.

| Revision number | Date    | Substantive changes                             |
| :-------------: | :-----: | :---------------------------------------------: |
| 0               | 03/2016 | Initial release                                 |
| 1               | 04/2016 | Updates for the Thread KW41 Alpha Release       |
| 2               | 08/2016 | Updates for the Thread KW41 Beta Release        |
| 3               | 09/2016 | Updates for the Thread KW41 GA Release          |
| 4               | 03/2017 | Updates for the Thread KW41 MCUX GA Release     |
| 5               | 01/2018 | Updates for the Thread KW41 Maintenance Release |

# **10. Copyright**

Information in this document is provided solely to enable system and software implementers to use NXP products. There are no express or implied copyright licenses granted hereunder to design or fabricate any integrated circuits based on the information in this document. NXP reserves the right to make changes without further notice to any products herein.

NXP makes no warranty, representation, or guarantee regarding the suitability of its products for any particular purpose, nor does NXP assume any liability arising out of the application or use of any product or circuit, and specifically disclaims any and all liability, including without limitation consequential or incidental damages. “Typical” parameters that may be provided in NXP data sheets and/or specifications can and do vary in different applications, and actual performance may vary over time. All operating parameters, including “typicals,” must be validated for each customer application by customer's technical experts. NXP does not convey any license under its patent rights nor the rights of others. NXP sells products pursuant to standard terms and conditions of sale, which can be found at the following address: nxp.com/SalesTermsandConditions.

NXP, the NXP logo, NXP SECURE CONNECTIONS FOR A SMARTER WORLD, All other product or service names are the property of their respective owners. ARM, AMBA, ARM Powered, are registered trademarks of ARM Limited (or its subsidiaries) in the EU and/or elsewhere. All rights reserved.

