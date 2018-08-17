# **Bluetooth® Low Energy Application Developer’s Guide**

- [**Bluetooth® Low Energy Application Developer’s Guide**](#bluetooth%C2%AE-low-energy-application-developers-guide)
- [1. 前言](#1-%E5%89%8D%E8%A8%80)
- [2. 先决条件](#2-%E5%85%88%E5%86%B3%E6%9D%A1%E4%BB%B6)
    - [2.1 RTOS任务队列和事件](#21-rtos%E4%BB%BB%E5%8A%A1%E9%98%9F%E5%88%97%E5%92%8C%E4%BA%8B%E4%BB%B6)
    - [2.2 GATT数据库](#22-gatt%E6%95%B0%E6%8D%AE%E5%BA%93)
    - [2.3 非易失性内存(NVM)访问](#23-%E9%9D%9E%E6%98%93%E5%A4%B1%E6%80%A7%E5%86%85%E5%AD%98nvm%E8%AE%BF%E9%97%AE)
- [3. 主机栈初始化和API](#3-%E4%B8%BB%E6%9C%BA%E6%A0%88%E5%88%9D%E5%A7%8B%E5%8C%96%E5%92%8Capi)
    - [3.1 主机任务初始化](#31-%E4%B8%BB%E6%9C%BA%E4%BB%BB%E5%8A%A1%E5%88%9D%E5%A7%8B%E5%8C%96)
    - [3.2 初始化主机的主要函数](#32-%E5%88%9D%E5%A7%8B%E5%8C%96%E4%B8%BB%E6%9C%BA%E7%9A%84%E4%B8%BB%E8%A6%81%E5%87%BD%E6%95%B0)
    - [3.3 HCI出入点](#33-hci%E5%87%BA%E5%85%A5%E7%82%B9)
    - [3.4 主机栈库和API可用性](#34-%E4%B8%BB%E6%9C%BA%E6%A0%88%E5%BA%93%E5%92%8Capi%E5%8F%AF%E7%94%A8%E6%80%A7)
    - [3.5 同步和异步函数](#35-%E5%90%8C%E6%AD%A5%E5%92%8C%E5%BC%82%E6%AD%A5%E5%87%BD%E6%95%B0)
    - [3.6 无线发射功率级别](#36-%E6%97%A0%E7%BA%BF%E5%8F%91%E5%B0%84%E5%8A%9F%E7%8E%87%E7%BA%A7%E5%88%AB)
- [4. 通用访问配置文件(GAP)层](#4-%E9%80%9A%E7%94%A8%E8%AE%BF%E9%97%AE%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6gap%E5%B1%82)
    - [4.1 中央设置](#41-%E4%B8%AD%E5%A4%AE%E8%AE%BE%E7%BD%AE)
        - [4.1.1 扫描](#411-%E6%89%AB%E6%8F%8F)
        - [4.1.2 发起和关闭连接](#412-%E5%8F%91%E8%B5%B7%E5%92%8C%E5%85%B3%E9%97%AD%E8%BF%9E%E6%8E%A5)
        - [4.1.3 配对和绑定](#413-%E9%85%8D%E5%AF%B9%E5%92%8C%E7%BB%91%E5%AE%9A)
    - [4.2 外设](#42-%E5%A4%96%E8%AE%BE)
        - [4.2.1 广告](#421-%E5%B9%BF%E5%91%8A)
        - [4.2.2 配对和绑定](#422-%E9%85%8D%E5%AF%B9%E5%92%8C%E7%BB%91%E5%AE%9A)
    - [4.3 LE数据包长度扩展](#43-le%E6%95%B0%E6%8D%AE%E5%8C%85%E9%95%BF%E5%BA%A6%E6%89%A9%E5%B1%95)
    - [4.4 增强型隐私特性](#44-%E5%A2%9E%E5%BC%BA%E5%9E%8B%E9%9A%90%E7%A7%81%E7%89%B9%E6%80%A7)
        - [4.4.1 简介](#441-%E7%AE%80%E4%BB%8B)
            - [4.4.1.1 可解析私有地址](#4411-%E5%8F%AF%E8%A7%A3%E6%9E%90%E7%A7%81%E6%9C%89%E5%9C%B0%E5%9D%80)
            - [4.4.1.2 非可解析私有地址](#4412-%E9%9D%9E%E5%8F%AF%E8%A7%A3%E6%9E%90%E7%A7%81%E6%9C%89%E5%9C%B0%E5%9D%80)
            - [4.4.1.3 多重身份解析密钥](#4413-%E5%A4%9A%E9%87%8D%E8%BA%AB%E4%BB%BD%E8%A7%A3%E6%9E%90%E5%AF%86%E9%92%A5)
        - [4.4.2 主机隐私](#442-%E4%B8%BB%E6%9C%BA%E9%9A%90%E7%A7%81)
        - [4.4.3 控制器隐私](#443-%E6%8E%A7%E5%88%B6%E5%99%A8%E9%9A%90%E7%A7%81)
            - [4.4.3.1 扫描和启动](#4431-%E6%89%AB%E6%8F%8F%E5%92%8C%E5%90%AF%E5%8A%A8)
            - [4.4.3.2 广告](#4432-%E5%B9%BF%E5%91%8A)
            - [4.4.3.3 连接](#4433-%E8%BF%9E%E6%8E%A5)


# 1. 前言

本文档介绍了如何在BLE应用程序中集成Bluetooth®LowEnergy（BLE）主机栈，并提供了最常用的API和代码示例的详细说明。

该文档还列出了BLE主机栈的先决条件和初始化，然后按照层和应用程序角色分组表示API，如下所述。

首先，通用访问配置文件（GAP）层根据设备的GAP角色分为两个部分：中央和外设。

通过代码示例解释了这两种设备的基本设置，例如如何准备设备以进行连接、如何将它们连接在一起以及配对和绑定过程。

接下来，通用属性配置文件（GATT）层介绍了两个连接设备之间数据传输所需的API。同样，根据设备的GATT角色，章节分为两个部分：客户端和服务器。

该文档进一步描述了应用程序中GATT数据库API的使用，以操纵GATT服务器数据库中的数据。

然后，文档展示了一种用户友好的静态构建GATT数据库的方法。该方法包括使用一组预定义的宏，应用程序可以使用这些宏在应用程序编译时构建数据库。

下一节包含关于如何构建自定义概要文件的说明。接下来的部分将专门介绍典型应用程序的结构。

此外，本文档还有一章专门介绍低功耗管理以及应用程序如何使用软硬件的低功耗模式。

下一节将介绍主机栈通过专用服务/配置文件提供的空中编程（OTAP）功能以及如何在应用程序中使用它们。本节还详细介绍了OTAP过程和引导装载程序应用程序中涉及的框架组件，引导程序应用程序在设备上进行映像的实际升级。

最后，文档有一个部分，描述了当主机栈在单独的处理器上运行时如何构建BLE应用程序。

------------------------------------------------------------------------------------------------------------------------

# 2. 先决条件

BLE主机栈库包含许多外部引用，应用程序必须定义这些引用以提供主机的全部功能。

如果不这样做，将在尝试构建应用程序二进制文件时导致链接错误。

## 2.1 RTOS任务队列和事件

这些任务队列在 **ble_host_tasks.h** 中声明，如下：
```c
/*! App to Host message queue for the Host Task */
extern msgQueue_t gApp2Host_TaskQueue;
/*! HCI to Host message queue for the Host Task */
extern msgQueue_t gHci2Host_TaskQueue; 
/*! Event for the Host Task Queue */
extern osaEventId_t gHost_TaskEvent;
```

有关主机所需的RTOS任务的详细信息，请参阅[主机任务初始化](#31-%E4%B8%BB%E6%9C%BA%E4%BB%BB%E5%8A%A1%E5%88%9D%E5%A7%8B%E5%8C%96)。

## 2.2 GATT数据库

由于内存效率的原因，主机栈不为GATT数据库分配内存。

相反，应用程序必须根据其需求和约束分配内存、定义和填充数据库。可以静态地、在应用程序编译时或动态地这样做。

无论应用程序如何创建GATT数据库，都必须定义 **gatt_database.h** 中的以下两个外部引用：
```c
/*! The number of attributes in the GATT Database. */
extern uint16_t gGattDbAttributeCount_c;
 
/*! Reference to the GATT database */
extern gattDbAttribute_t gattDatabase[];
```

属性模板（gattDbAttribute_t）定义如下：
```c
typedefstruct gattDbAttribute_tag {
    uint16_t handle ; 
/*!< Attribute handle - cannot be 0x0000; attribute handles need not be consecutive, but must be strictly increasing. */
    uint16_t permissions ; 
/*!< Attribute permissions as defined by ATT. */
    uint32_t uuid ; 
/*!< The UUID should be read according to the gattDbAttribute_t.uuidType member: for 2-byte and 4-byte UUIDs, this contains the value of the UUID; for 16-byte UUIDs, this is a pointer to the allocated 16-byte array containing the UUID. */
    uint8_t * pValue ; 
/*!< Pointer to allocated value array. */
    uint16_t valueLength ; 
/*!< Size of the value array. */
    uint16_t uuidType : 2; 
/*!< Identifies the length of the UUID; the 2-bit values are interpreted according to the bleUuidType_t enumeration. */
    uint16_t maxVariableValueLength : 10; 
/*!< Maximum length of the attribute value array; if this is set to 0, then the attribute's length (valueLength) is fixed and cannot be changed. */
} gattDbAttribute_t ;
```

## 2.3 非易失性内存(NVM)访问

主机栈包含一个内部设备信息管理，它依赖于访问非易失性内存来存储和加载绑定的设备数据。

应用程序开发者通过定义三个函数和一个变量来确定NVM访问机制。函数必须首先对信息进行预处理，然后执行标准的NVM操作（擦除、写入、读取）。声明如下：
```c
extern void App_NvmErase
(
    uint8_t mEntryIdx
);
extern void App_NvmWrite
(
    uint8_t mEntryIdx,
    void*   pBondHeader 
    void*   pBondDataDynamic,
    void*   pBondDataStatic,
    void*   pBondDataDeviceInfo,    
    void*   pBondDataDescriptor,
   uint8_t mDescriptorIndex
);
extern void App_NvmRead
(
    uint8_t mEntryIdx,
    void*   pBondHeader 
    void*   pBondDataDynamic,
    void*   pBondDataStatic,
    void*   pBondDataDeviceInfo,     
    void*   pBondDataDescriptor,
   uint8_t mDescriptorIndex
);
```

设备信息被划分成几个部件，以确保软件磨损均衡机制也能得到最优的使用。部件大小是固定的（在 **ble_constant.h** 中定义），并且具有以下含义：

| API pointer to bond component                                        | Component size (ble_constants.h) | Description                                                                  |
| :------------------------------------------------------------------- | :------------------------------- | :--------------------------------------------------------------------------- |
| pBondHeader: points to a bleBondIdentityHeaderBlob_t element         | gBleBondIdentityHeaderSize_c     | Bonding information which is sufficient to identify a bonded device          |
| pBondDataDynamic: points to a bleBondDataDynamicBlob_t element       | gBleBondDataDynamicSize_c        | Bonding information that might change frequently                             |
| pBondDataStatic: points to a bleBondDataStaticBlob_t element         | gBleBondDataStaticSize_c         | Bonding information that is unlikely to change frequently                    |
| pBondDataDeviceInfo: points to a bleBondDataDeviceInfoBlob_t element | gBleBondDataDeviceInfoSize_c     | Additional bonding information that can be accessed using the host stack API |
| pBondDataDescriptor: points to a bleBondDataDescriptorBlob_t element | gBleBondDataDescriptorSize_c     | Bonding information used to store one CCCD                                   |

应用程序开发者不需要关心绑定信息的格式，因为这是由主机栈处理的。每个绑定数据槽必须包含一个绑定头blob、一个动态数据blob、一个静态数据blob、一个设备信息blob和一个等于 gcGapMaximumSavedCccds_c（ **ble_constant.h** ）的描述符blob数组。一个槽由 mEntryIdx 参数唯一标识。一个描述符由 mEntryIdx - mDescriptorIndex 对唯一标识。

> PS：CCCD - Client Characteristic Configuration Descriptor

如果作为参数传递的一个或多个指针为空，则必须忽略从绑定槽的对应blob的读写操作。擦除函数必须清除条目索引指定的整个绑定数据槽。

上述函数的当前实现使用框架NVM模块或RAM缓冲区。有关NVM配置和功能的其他详细信息可以在 **Connectivity Framework Reference Manual** 中找到。

要启用NVM机制，请确保：
* 将 gAppUseNvm_d（ **app_preinclude.h** ）设置为 1
* 工具链链接器选项中的 gUseNVMLink_d = 1

如果 gAppUseNvm_d 设置为0，则所有绑定数据将存储在RAM中，并且可以访问，直到复位或重新上电。

> NOTE：如果 gAppUseNvm_d 设置为1，则默认的NVM模块配置将应用于 **app_preinclude.h** 文件中。

------------------------------------------------------------------------------------------------------------------------

# 3. 主机栈初始化和API

## 3.1 主机任务初始化

应用程序开发者需要将主机任务配置为主机栈需求的一部分。任务是运行所有主机层的环境（GAP、GATT、ATT、L2CAP、SM、GATTDB）

任务函数的原型在 **ble_host_tasks.h** 文件中：
```c
void Host_TaskHandler(void * args);
```

应该在应用程序的任务代码中以NULL作为参数调用它。

应用程序开发者需要定义任务事件和队列，如[RTOS任务队列和事件中所述](#21-rtos%E4%BB%BB%E5%8A%A1%E9%98%9F%E5%88%97%E5%92%8C%E4%BA%8B%E4%BB%B6)。

主机任务的优先级始终高于控制器任务。优先级值通过 gHost_TaskPriority_c（ **ble_host_task_config.h** ）和 gControllerTaskPriority_c（ **ble_controller_task_config.h** ）来配置。注意，改变这些值会对BLE栈产生重大的影响。

优先级别是根据OS抽象（OSA）优先级定义的，其中0是最高优先级，15是最低优先级。有关更多信息，请参阅 **Connectivity Framework Reference Manual**。注意，RTOS特定的优先级可能因操作系统而异。

## 3.2 初始化主机的主要函数

在平台设置完成并启动所有RTOS任务之后，必须初始化主机栈。

需要调用的函数位于 **ble_general.h** 文件中，并具有以下原型：
```c
bleResult_t Ble_HostInitialize
(
    gapGenericCallback_t genericCallback,
    hciHostToControllerInterface_t hostToControllerInterface
);
```

genericCallback 是应用程序已装入的主要回调。它接收来自GAP层的大多数事件，这些事件称为通用事件。通用事件具有一个类型（请参阅 gapGenericEventType_t）和根据事件类型（联合体）的数据。

hostToControllerInterface 是主机栈的HCI出口点。这是主机每次尝试向LE控制器发送HCI消息时调用的函数。

通过 gInitializationComplete_c 通用事件在 genericCallback 中发出主机栈初始化的完成信号。

在接收到该事件之后，可以启动主应用程序逻辑。

![Figure 1. BLE Host Stack overview](../Pic/BLE%20Application%20Developer's%20Guide-Figure1.jpg)

## 3.3 HCI出入点

主机栈的HCI入口点位于 **ble_general.h** 文件中的第二个函数：
```c
void Ble_HciRecv
(
    hciPacketType_t packetType,
    void* pPacket,
    uint16_t packetSize
);
```

这是应用程序必须调用以将HCI消息插入到主机中的函数。

因此，Ble_Initialize 函数中的 Ble_HciRecv 函数和 hostToControllerInterface 参数直接（如果控制器软件和主机运行在同一芯片上）表示需要连接到LE控制器的两个点（参见[初始化主机的主要函数](#32-%E5%88%9D%E5%A7%8B%E5%8C%96%E4%B8%BB%E6%9C%BA%E7%9A%84%E4%B8%BB%E8%A6%81%E5%87%BD%E6%95%B0)）或通过一个物理接口（例如UART）。

## 3.4 主机栈库和API可用性

本文档中引用的所有API均可在中央和外设库中获得。例如，**ble_host_lib.a** 是一个功能齐全的库，完整地支持GAP级别的中央和外设API，以及GATT级别的客户端和服务器API。

但是，某些应用程序可能针对内存受限的设备，并不需要完整的支持。为了减少代码大小和RAM利用率，还提供了另外两个库：
* **ble_host_peripheral_lib.a**
    * 只支持用于GAP外设和GAP广播者角色的API
    * 只支持用于GATT服务器角色的API
* **ble_host_peripheral_lib.a**
    * 只支持用于GAP中央和GAP观察者角色的API
    * 只支持用于GATT客户端角色的API

若果尝试使用其不支持的API（例如，使用 **ble_host_peripheral_lib.a** 调用 Gap_Connect），API会返回 gBleFeatureNotSupported_c 错误代码。

> NOTE：有关API支持的明确信息，请参阅 **Bluetooth Low Energy Host StackAPI Reference Manual**。每个函数文档都在备注部分包含此信息。

## 3.5 同步和异步函数

绝大多数GAP和GATT的API都是异步执行的。调用这些函数会生成RTOS消息，并且位于主机任务消息队列中。

因此，这些API的实际结果将在由应用程序安装的特定回调触发的事件中发出信号。有关每个API触发的事件的特定信息，请参阅 **Bluetooth Low Energy Host StackAPI Reference Manual**。

但是，有一些API会立即执行（同步）。在 **Bluetooth Low Energy Host StackAPI Reference Manual** 的每个函数文档中明确提到了这一点。

如果没有提到任何内容，则该API是异步的。

## 3.6 无线发射功率级别

控制器接口包含一个API，可用于将无线发射功率设置为不同的级别。

对于广告和连接通道，可以使用以下宏设置不同的功率级别：
```c
#define Controller_SetAdvertisingTxPowerLevel(level) \
Controller_SetTxPowerLevel(level,gAdvTxChannel_c)
```
和
```c
#define Controller_SetConnectionTxPowerLevel(level) \
Controller_SetTxPowerLevel(level,gConnTxChannel_c)
```

数字功率级别均匀分布在最小和最大输出功率值之间（以dBm表示）。更多信息请参考 **silicon datasheet**。

------------------------------------------------------------------------------------------------------------------------

# 4. 通用访问配置文件(GAP)层

GAP层管理连接，安全性和绑定设备。

GAP层API构建在主机控制器接口（HCI）、安全管理器协议（SMP）和设备数据库之上。

GAP定义了BLE设备在BLE系统中可能具有的四种角色（见 4.1.3 节的 Table 1. GAP Security Modes and Levels）：
* 中央
    * 扫描广告者（外设和广播者）
    * 发起与外设的连接；在链路层（LL）级别为Master
    * 通常充当GATT客户端，但其本身也可以包含GATT数据库
* 外设
    * 广告和接受来自中央的连接请求；在链路层（LL）级别为Slave
    * 通常包含GATT数据库并充当GATT服务器，但也可能是客户端
* 观察者
    * 扫描广告者，但不发起连接；传输是可选的
* 广播者
    * 广告，但不接受来自中央的连接请求；接收是可选的

![Figure 2. GAP Topology](../Pic/BLE%20Application%20Developer's%20Guide-Figure2.jpg)

## 4.1 中央设置

通常，中央必须开始扫描以查找外设。当中央扫描了它想要连接的外设时，它会停止扫描并发起与该外设的连接。建立连接后，如果外设需要，它可以进行配对，或者如果两个设备在过去已经绑定，则可以直接加密链路。

### 4.1.1 扫描

中央设备的最基本设置从扫描开始，由 **gap_interface.h** 中的以下函数执行：
```c
bleResult_t Gap_StartScanning
(
    gapScanningParameters_t * pScanningParameters,
    gapScanningCallback_t scanningCallback
);
```

如果 pScanningParameters 指针为NULL，则使用当前设置的参数。如果在设备上电后未设置任何参数，则使用标准默认值：
```c
#define gGapDefaultScanningParameters_d \
{ \
    /* type */              gGapScanTypePassive_c, \
    /* interval */          gGapScanIntervalDefault_d, \
    /* window */            gGapScanWindowDefault_d, \
    /* ownAddressType */    gBleAddrTypePublic_c, \
    /* filterPolicy */      gScanAll_c \
}
```

定义非默认扫描参数的最简单方法是使用上述默认值初始化 gapScanningParameters_t 结构，并只更改所需的字段。

例如，要执行主动扫描并仅扫描白名单中的设备，可以使用以下代码：
```c
gapScanningParameters_t scanningParameters = gGapDefaultScanningParameters_d;
scanningParameters.type = gGapScanTypeActive_c;
scanningParameters.filterPolicy = gScanWhiteListOnly_c;
Gap_StartScanning(&scanningParameters, scanningCallback);
```

scanningCallback 通过GAP层与扫描有关的信号事件触发。

最重要的事件是 gDeviceScanned_c 事件，每次扫描到广告设备时都会触发该事件。此事件的数据包含有关广告者的信息：
```c
typedefstruct gapScannedDevice_tag {
    bleAddressType_t                 addressType;
    bleDeviceAddress_t               aAddress;
    int8_t                           rssi;
    uint8_t                          dataLength;
    uint8_t *                        data;
    bleAdvertisingReportEventType_t  advEventType;
} gapScannedDevice_t;
```

如果此信息表示为中央想要连接的已知外设，则后者必须停止扫描和连接到该外设。

要停止扫描，请调用此函数：
```c
bleResult_t Gap_StopScanning (void);
```

默认情况下，GAP层配置为使用 gDeviceScanned_c 事件类型将所有已扫描到的设备报告给应用程序。但是，某些用例可能需要执行特定的GAP发现程序，其中广告报告必须通过广告数据中的 Flags AD 值进行过滤。其他用例要求主机栈在扫描特定设备时自动发起连接。

要启用基于 Flags AD 值的过滤或设置自动连接的设备地址，必须在扫描开始之前调用以下函数：
```c
bleResult_tGap_SetScanMode
(
    gapScanMode_t scanMode,
    gapAutoConnectParams_t* pAutoConnectParams
);
```

扫描模式的默认值为 gNoDiscovery_c，它会报告所有数据包，而不管其内容如何，​​并且不执行任何自动连接。

要启用有限发现，必须使用 gLimitedDiscovery_c 值，而 gGeneralDiscovery_c 值激活通用发现。

要在扫描特定设备时启用自动连接，必须设置 gAutoConnect_c 值，在这种情况下，pAutoConnectParams 参数必须指向保存目标设备地址的结构以及主机用于这些设备的连接参数。

### 4.1.2 发起和关闭连接

要连接到已扫描的外设，请从 gDeviceScanned_c 事件数据中提取其地址和地址类型，停止扫描并调用以下函数：
```c
bleResult_t Gap_Connect
(
    gapConnectionRequestParameters_t * pParameters,
    gapConnectionCallback_t connCallback
);
```

创建连接参数结构的简单方法是使用默认值初始化它，然后仅更改需要的字段。默认结构定义如下：
```c
#define gGapDefaultConnectionRequestParameters_d \
{ \
    /* scanInterval */          gGapScanIntervalDefault_d, \
    /* scanWindow */            gGapScanWindowDefault_d, \
    /* filterPolicy */          gUseDeviceAddress_c, \
    /* ownAddressType */        gBleAddrTypePublic_c, \
    /* peerAddressType */       gBleAddrTypePublic_c, \
    /* peerAddress */           { 0, 0, 0, 0, 0, 0 }, \
    /* connIntervalMin */       gGapDefaultMinConnectionInterval_d, \
    /* connIntervalMax */       gGapDefaultMaxConnectionInterval_d, \
    /* connLatency */           gGapDefaultConnectionLatency_d, \
    /* supervisionTimeout */    gGapDefaultSupervisionTimeout_d, \
    /* connEventLengthMin */    gGapConnEventLengthMin_d, \
    /* connEventLengthMax */    gGapConnEventLengthMax_d \
}
```

在以下示例中，中央扫描一个具有已知地址的特定心率传感器。当它找到它时，它立即连接到它。
```c
static bleDeviceAddress_t heartRateSensorAddress = { 0xa1, 0xb2, 0xc3, 0xd4, 0xe5, 0xf6 };
static bleAddressType_t hrsAddressType = gBleAddrTypePublic_c;
static bleAddressType_t ownAddressType = gBleAddrTypePublic_c;
voidgapScanningCallback( gapScanningEvent_t * pScanningEvent)
{
    switch (pScanningEvent-> eventType )
    {
        /* ... */
        casegDeviceScanned_c:
        {
            if (hrsAddressType == pScanningEvent -> eventData.scannedDevice.addressType && 
                Ble_DeviceAddressesMatch(heartRateSensorAddress, pScanningEvent-> eventData.scannedDevice.aAddress ))
            {
                gapConnectionRequestParameters_t connReqParams = gGapDefaultConnectionRequestParameters_d; 
                connReqParams.peerAddressType = hrsAddressType;
                Ble_CopyDeviceAddress(connReqParams.peerAddress , heartRateSensorAddress); 
                connReqParams.ownAddressType = ownAddressType;
                bleResult_t result = Gap_StopScanning();
                if (gBleSuccess_c != result)
                {
                    /* Handle error */
                }
                else
                {
                    /* There is no need to wait for the gScanStateChanged_c event because
                     * the commands are queued in the host task
                     * and executed consecutively. */
                    result = Gap_Connect(&connReqParams, connectionCallback);
                    if (gBleSuccess_c != result)
                    {
                        /* Handle error */
                    }
                }
            }
            break;
        }
        /* ... */
    }
}
```

connCallback 由GAP触发，以发送与活动连接相关的所有事件。它有以下原型：
```c
typedef void (* gapConnectionCallback_t )
(
    deviceId_t deviceId,
    gapConnectionEvent_t * pConnectionEvent
);
```

在这个回调中应该监听的第一个事件是 gConnEvtConnected_c 事件。如果应用程序决定在产生此事件之前删除连接建立，它应该调用以下宏：
```c
#define Gap_CancelInitiatingConnection()\
    Gap_Disconnect(gCancelOngoingInitiatingConnection_d)
```

这是非常有用的，例如，当应用程序选择对连接请求使用限时计时器时。

在接收到 gConnEvtConnected_c 事件时，应用程序可以继续从事件数据（pConnectionEvent->event.connectedEvent）中提取必要的参数。要保存的最重要的参数是 deviceId。

deviceId 是一个唯一的8位无符号整数，用于识别后续GAP和GATT的API调用的活动连接。与某个连接相关的所有功能都需要deviceId 参数。例如，要断开连接，请调用此函数：
```c
bleResult_t Gap_Disconnect
(
    deviceId_t deviceId
);
```

### 4.1.3 配对和绑定

用户连接到外设后，使用以下功能检查此设备过去是否已绑定：
```c
bleResult_t Gap_CheckIfBonded
(
    deviceId_t deviceId,
    bool_t * pOutIsBonded
);
```

如果有，可以通过以下方式请求链路加密：
```c
bleResult_t Gap_EncryptLink
(
    deviceId_t deviceId,
);
```

如果链路加密成功，则会触发 gConnEvtEncryptionChanged_c 连接事件。否则，将收到 gConnEvtAuthenticationRejected_c 事件，并将 rejectReason 事件数据参数设置为 gLinkEncryptionFailed_c。

另一方面，如果这是一个新设备（未绑定），可以启动配对，如下所示：
```c
bleResult_t Gap_Pair
(
    deviceId_t deviceId,
    gapPairingParameters_t * pPairingParameters
);
```

配对参数如下所示：
```c
typedef struct gapPairingParameters_tag {
    bool_t                       withBonding;
    gapSecurityModeAndLevel_t    securityModeAndLevel;
    uint8_t                      maxEncryptionKeySize;
    gapIoCapabilities_t          localIoCapabilities;
    bool_t                       oobAvailable;
    gapSmpKeyFlags_t             centralKeys;
    gapSmpKeyFlags_t             peripheralKeys;
    bool_t                       leSecureConnectionSupported;
    bool_t                       useKeypressNotifications;
} gapPairingParameters_t;
```

参数的名称是自解释的。如果中央想要绑定，那么 withBonding 标志应设置为TRUE。

对于安全模式和级别，GAP层将其定义如下：（M代表安全模式，L代表级别）
* M1L1 代表无安全要求
* 除了 M1L1，其他 M1 需要加密，而 M2 需要数据签名
* M1L2 和 M2L1 不需要身份验证（它们允许仅工作配对，没有MITM保护），而 M1L3 和 M2L2 需要身份验证（必须使用PIN或OOB数据配对，以提供MITM保护）
* 从蓝牙规范4.2开始，OOB配对仅在某些条件下提供MITM保护。如果OOB数据交换功能通过专用API提供MITM保护，则应用程序必须通知栈
* M1L4 保留给使用LE安全连接配对方法进行身份验证的配对（带有MITM保护）
* 如果使用LE安全连接配对方法但它不提供MITM保护，则配对将以 M1L2 完成。

> PS：仅工作 - 仅工作（Just Works）方式主要针对没有输入方式的设备。

> PS：MITM - 在密码学和计算机安全领域中，中间人攻击（英语：Man-in-the-middle attack，缩写：MITM）是指攻击者与通讯的两端分别创建独立的联系，并交换其所收到的数据，使通讯的两端认为他们正在通过一个私密的连接与对方直接对话，但事实上整个会话都被攻击者完全控制。在中间人攻击中，攻击者可以拦截通讯双方的通话并插入新的内容。

> PS：OOB数据 - 在计算机网络中，带外数据（out-of-band data）是通过独立于主带内数据流的流传输的数据。带外数据机制提供概念上独立的信道，允许通过该机制发送的任何数据与带内数据分开。应该提供带外数据机制作为数据信道和传输协议的固有特性，而不是要求建立单独的信道和端点。

![Table 1. GAP Security Modes and Levels](../Pic/BLE%20Application%20Developer's%20Guide-Table1.jpg)

centralKeys 应该为应用程序中可用的所有密钥设置标志。如果中央使用私有解析地址，则IRK是强制性的，而如果中央想要使用数据签名，则必须使用CSRK。LTK是由外设提供的，只有当中央打算在未来的重新连接（GAP角色更改）中成为外设时才应包括LTK。

peripheralKeys 应遵循相同的准则。如果要执行加密，LTK是强制性的，而如果外设使用私有解析地址，则应该请求对端的IRK。

有关密钥分发的详细指南，请参阅  4.1.3 节的 Table 2. Key Distribution guidelines。

前三行是配对参数（centralKeys 和 peripheralKeys）和使用 Gap_SendSmpKeys 分发密钥的指南。

如果执行LE安全连接配对（BLE 4.2），则LTK在内部生成，因此设备将忽略配对参数的密钥分发字段中的相应位。

如果IRK也是分发的（其标志已在配对参数中设置），则应分发身份地址。因此，只能通过询问IRK（它在 gapSmpKeyFlags_t 结构中没有单独的标志）来“询问”它，因此是N/A。

分发密钥的协商如下：
* 在SMP配对请求（由 Gap_Pair 启动）中，中央为要分发（centralKeys）和接收（peripheralKeys）的密钥设置标志  
 ![Table 2. Key Distribution guidelines](../Pic/BLE%20Application%20Developer's%20Guide-Table2.jpg)
* 外设检查这两个分发，并且在执行它认为必要的任何更改后必须发送SMP配对响应（由 Gap_AcceptPairingRequest 启动）。外设只允许将一些中央设置为1的标志设置为0，但不允许相反。例如，它不能 请求/分发 中央没有 提供/要求 的密钥。如果外设与中央的分发相反，它可以通过使用 Gap_RejectPairing 函数拒绝配对
* 中央检查配对响应中的更新的分发。如果它与外设所做的更改相反，它可以拒绝配对（Gap_RejectPairing）。否则，配对继续，并且在密钥分发阶段（gConnEvtKeyExchangeRequest_c 事件），只有最终协商的密钥包含在与 Gap_SendSmpKeys 一起发送的密钥结构中
* 对于LE安全连接（两个设备都在配对请求和配对响应数据包的 AuthReq 字段中设置 SC 位），LTK不会被分发，它将被生成，并且配对响应包的 Inittiator Key Distribution 和 Responder Key Distribution 字段中的相应位应设置为0。

如果使用LE安全连接配对（BLE 4.2），并且需要交换OOB数据，则应用程序必须通过调用 Gap_LeScGetLocalOobData 函数从主机栈中获取本地LE SC OOB数据。数据包含在通用的 gLeScLocalOobData_c 事件中。

在以下情况下刷新本地LE SC OOB数据：
* Gap_LeScRegeneratePublicKey 函数被调用（gLeScPublicKeyRegenerated_c 通用事件也作为这个API的结果而产生）
* 设备被重置(这也会导致公钥重新生成)

如果配对继续，可能会发生以下连接事件：
* 请求事件
    * gConnEvtPasskeyRequest_c：配对需要PIN；应用程序必须使用 Gap_EnterPasskey(deviceId, passkey) 进行响应
    * gConnEvtOobRequest_c：如果配对开始时双方都将 oobAvailable 设置为TRUE；应用程序必须使用 Gap_ProvideOob(deviceId, oob) 进行响应
    * gConnEvtKeyExchangeRequest_c：配对已达到密钥交换阶段；应用程序必须使用 Gap_SendSmpKeys(deviceId, smpKeys) 进行响应
    * gConnEvtLeScOobDataRequest_c：栈请求从对端（r，Cr 和 Addr）接收的LE SC OOB数据；应用程序必须使用 Gap_LeScSetPeerOobData(deviceId, leScOobData) 进行响应
    * gConnEvtLeScDisplayNumericValue_c：栈请求显示和确认LE SC数值比较值；应用程序必须使用 Gap_LeScValidateNumericValue(deviceId, ncvValidated) 进行响应
* 信息事件
    * gConnEvtKeysReceived_c：密钥交换阶段完成；密钥自动保存在内部设备数据库中，并提供给应用程序进行即时检查；应用程序不必将密钥保存在NVM存储中，因为如果双方都将 withBonding 设置为TRUE，则会在内部完成
    * gConnEvtAuthenticationRejected_c：对端设备拒绝配对；事件数据的 rejectReason 参数指示外设与配对参数不一致的原因（它不能是 gLinkEncryptionFailed_c，因为该原因是为链路加密失败保留的）
    * gConnEvtPairingComplete_c：配对过程已成功完成，或者在SMP数据包交换期间可能发生错误；请注意，这与 gConnEvtKeyExchangeRequest_c 事件不同；后者表示配对被对端拒绝，而前者则用于因SMP数据包交换而导致的失败
    * gConnEvtLeScKeypressNotification_c：栈通知应用程序在 Passkey Entry Pairing Method 期间已收到远程 SMP Keypress Notification

链路加密或配对成功完成后，中央可以立即开始使用GATT API交换数据。

![Figure 3. Central pairing flow – APIs and eventsGap_RejectPairing may be called on any pairing event](../Pic/BLE%20Application%20Developer's%20Guide-Figure3.jpg)

## 4.2 外设

外设开启广告，等待其他中央的扫描和连接请求。

### 4.2.1 广告

在开始广告之前，应配置广告参数。否则，使用以下默认值：
```c
#define gGapDefaultAdvertisingParameters_d \
{ \
    /* minInterval */            gGapAdvertisingIntervalDefault_c, \
    /* maxInterval */            gGapAdvertisingIntervalDefault_c, \
    /* advertisingType */        gConnectableUndirectedAdv_c, \
    /* addressType */            gBleAddrTypePublic_c, \
    /* directedAddressType */    gBleAddrTypePublic_c, \
    /* directedAddress */        {0, 0, 0, 0, 0, 0}, \
    /* channelMap */             (gapAdvertisingChannelMapFlags_t) (gGapAdvChanMapFlag37_c | gGapAdvChanMapFlag38_c | gGapAdvChanMapFlag39_c), \
    /* filterPolicy */           gProcessAll_c \
}
```

要设置不同的广告参数，应分配 gapAdvertisingParameters_t 结构并使用默认值进行初始化。然后，可以修改必要的字段。

之后，应调用以下函数：
```c
bleResult_t Gap_SetAdvertisingParameters
(
    gapAdvertisingParameters_t * pAdvertisingParameters
);
```

应用程序应该监听 gAdvertisingParametersSetupComplete_c 通用事件。

接下来，应该配置广告数据，并且如果广告类型支持主动扫描，则还应该配置扫描响应数据。如果未配置其中任何一个，则默认为空数据。

用于配置广告 和/或 扫描响应数据的函数如下所示：
```c
bleResult_t Gap_SetAdvertisingData
(
    gapAdvertisingData_t * pAdvertisingData,
    gapScanResponseData_t * pScanResponseData
);
```

这两个指针中的任何一个都可以是NULL，在这种情况下它们将被忽略（相应的数据保留为先前配置的，如果从未设置过，则为空），但不能同时忽略这两个指针。

应用程序应该监听 gAdvertisingDataSetupComplete_c 通用事件。

完成所有必要的设置后，可以使用此函数启动广告：
```c
bleResult_t Gap_StartAdvertising
(
    gapAdvertisingCallback_t advertisingCallback,
    gapConnectionCallback_t connectionCallback
);
```

advertisingCallback 用于接收广告事件（广告状态更改或广告命令失败），而 connectionCallback 仅在广告期间已建立连接时使用。

该连接回调与中央调用 Gap_Connect 函数时使用的回调相同 。

如果中央发起与此外设的连接，则会触发 gConnEvtConnected_c 连接事件。

要在外设尚未收到任何连接请求时停止广告，请使用此函数：
```c
bleResult_t Gap_StopAdvertising (void);
```

在外设进入连接后不应调用此函数。

### 4.2.2 配对和绑定

在与中央建立连接后，外设在安全方面的作用是被动的。中央负责启动配对过程，或者如果设备过去已经绑定，则使用共享LTK加密链路。

如果中央试图在不进行身份验证的情况下访问敏感数据，外设会使用适当的错误代码（身份验证不足，加密不足，授权不足等）发送错误响应（在ATT级别），从而向中央指示它需要执行安全程序。

所有安全检查都由GAP模块内部执行，并自动发送安全错误响应。应用程序开发者需要做的就是注册安全需求。

首先，在构建GATT数据库时（请参阅[创建GATT数据库]()），敏感属性应该在其访问权限中内置安全性（例如，只读/带有身份验证读取/带有身份验证写入/具有授权写入，等等）。

其次，如果GATT数据库除了在属性权限中已经指定的安全性之外还需要额外的安全性（例如，某些服务在某些情况下需要更高的安全性），则必须调用以下函数：
```c
bleResult_t Gap_RegisterDeviceSecurityRequirements
(
    gapDeviceSecurityRequirements_t * pSecurity
);
```

该参数是一个指向结构的指针，该结构包含设备主要安全设置和服务特定的安全设置。所有这些安全性要求都是指向 gapSecurityRequirements_t 结构的指针。要忽略的指针应设置为NULL。

虽然外设不会启动任何类型的安全程序，但它可以向中央通报其安全要求。这通常在连接之后立即执行，以避免为请求而交换无用的数据包（因安全性不足可能被拒绝）。

通知在SMP级别通过从设备安全请求包被执行。要使用它，请提供以下GAP API：
```c
bleResult_t Gap_SendSlaveSecurityRequest
(
    deviceId_t deviceId,
    bool_t bondAfterPairing,
    gapSecurityModeAndLevel_t securityModeLevel
);
```

bondAfterPairing 参数向中央指示此外设是否可以绑定，securityModeLevel 通知中央对其进行配对时所需的安全模式和级别。有关安全模式和级别的说明，请参阅第 4.1.3 节，其由GAP模块定义。

此请求不需要中央的答复，也不需要中央立即采取任何行动。中央可以简单地选择忽略从设备安全请求。

如果两个设备已经绑定过，则外设应该期望接收 gConnEvtLongTermKeyRequest_c 连接事件（除非执行LE安全连接配对，如BLE 4.2中所指定的），这意味着中央也识别为绑定，而不是配对，它直接使用先前共享的LTK加密链路。此时，本地LE控制器请求主机提供在配对期间交换的相同的LTK。

当设备先前已配对过时，除了外设的LTK，还会发送EDIV（2字节）和RAND（8字节）值（它们由SMP定义）。因此，在向控制器提供密钥之前，应用程序应检查这两个值是否与 gConnEvtLongTermKeyRequest_c 事件中接收的值匹配。如果他们这样做，应用程序应回复：
```c
bleResult_t Gap_ProvideLongTermKey
(
    deviceId_t deviceId,
    uint8_t * aLtk,
    uint8_t ltkSize
);
```

LTK的大小不能超过最大值：16。

如果EDIV和RAND值不匹配，或者外设无法识别绑定，则可以拒绝加密请求：
```c
bleResult_t Gap_DenyLongTermKey
(
    deviceId_t deviceId
);
```

如果使用LE SC配对，则LTK由主机栈内部生成，并且在绑定后链路加密期间不会从应用程序请求它。在此方案中，仅通过 gConnEvtEncryptionChanged_c 连接事件通知应用程序链路加密 。

如果设备未绑定，则外设应该期望接收 gConnEvtPairingRequest_c，这指示中央已启动了配对。

如果应用程序接受配对参数（请参阅[配对和绑定](#413-%E9%85%8D%E5%AF%B9%E5%92%8C%E7%BB%91%E5%AE%9A)以获取详细说明），它可以回复：
```c
bleResult_t Gap_AcceptPairingRequest
(
    deviceId_t deviceId,
    gapPairingParameters_t * pPairingParameters
);
```

这时，外设发送自己的配对参数，如SMP所定义。

发送此响应后，应用程序应该期望接收与中央相同的配对事件（请参阅[配对和绑定](#413-%E9%85%8D%E5%AF%B9%E5%92%8C%E7%BB%91%E5%AE%9A)），但有一个例外：如果应用程序在连接之前通过调用如下API设置了配对的密钥（PIN），则 gConnEvtPasskeyRequest_c 事件不会被调用：
```c
bleResult_t Gap_SetLocalPasskey
(
    uint32_t passkey
);
```

这样做的原因通常是外设具有一个静态密钥PIN，它仅分发给信任的设备。如果由于任何原因，外设必须动态更改PIN，它可以在配对开始之前调用上述函数（例如，在使用 Gap_AcceptPairingRequest 发送配对响应之前）。

如果外设应用程序从不调用 Gap_SetLocalPasskey，则 gConnEvtPasskeyRequest_c 事件将照常发送到应用程序。

外设可以使用以下API来拒绝配对过程：
```c
bleResult_t Gap_RejectPairing
(
    deviceId_t                       deviceId,
    gapAuthenticationRejectReason_t  reason
);
```

reason 应说明拒绝配对的原因。gLinkEncryptionFailed_c 值保留用于 gConnEvtAuthenticationRejected_c 连接事件，以指示链路加密失败而不是配对失败。因此，它并不能作为配对拒绝的原因。

Gap_RejectPairing 函数不仅可以在接收到配对请求后被调用，还可以在配对过程中被调用（在处理配对事件时或异步，如果因某种原因外设决定中断配对）。这也适用于中央。

![Figure 4. Peripheral pairing flow – APIs and eventsGap_RejectPairing may be called on any pairing event](../Pic/BLE%20Application%20Developer's%20Guide-Figure4.jpg)

对于中央和外设，绑定是在内部执行的，不是应用程序的关注点。应用程序通过 gConnEvtPairingComplete_c 事件参数被告知是否发生了绑定。

## 4.3 LE数据包长度扩展

该新特性将最大数据通道有效负载长度从27个字节扩展到251个字节（8-bit）。

建立连接后，链路层会立即自动完成长度管理。栈传递默认值，用于表示最大传输的有效负载字节数和最大包传输时间，应用程序在编译时在 **ble_globals.c** 中配置该默认值：
```c
#ifndef gBleDefaultTxOctets_c
#define gBleDefaultTxOctets_c 0x00FB
#endif

#ifndef gBleDefaultTxTime_c
#define gBleDefaultTxTime_c 0x0848
#endif
```

设备可以在连接时随时更新数据长度。触发此机制的函数如下：
```c
bleResult_t Gap_UpdateLeDataLength
(
    deviceId_t deviceId,
    uint16_t   txOctets,
    uint16_t   txTime
);
```

在执行该过程之后，将触发 gConnEvtLeDataLengthChanged_c 连接事件，其事件数据中包含最大传输的有效负载字节数和最大包传输时间。如果远程设备启动该过程，则该事件是发送事件。该过程详述如下：

![Figure 5. Data Length Update Procedure](../Pic/BLE%20Application%20Developer's%20Guide-Figure5.jpg)

## 4.4 增强型隐私特性

### 4.4.1 简介

蓝牙4.2主机栈引入了对增强型隐私特性的支持。

可以在主机或控制器中启用隐私：
* 主机隐私内容包括：
    * 定期在主机内重新生成随机地址（可解析或非可解析私有地址）并将其应用到控制器中
    * 保持主机中的对端IRK列表并尝试解决任何传入的RPA
* 控制器隐私（蓝牙4.2引入的），包括在控制器中写入本地IRK，以及所有已知的对端IRK，并让控制器执行硬件、全自动IRK生成和解析

> PS：RPA - 可解析私有地址（Resolvable Private Address）

可以随时启用主机隐私或控制器隐私。当在在另一个隐私正在进行时试图启用一个隐私会生成 gBleInvalidState_c 错误。当尝试两次启用相同的隐私类型时，或者在未启用隐私时尝试禁用隐私，都会会返回相同的错误。

#### 4.4.1.1 可解析私有地址

可解析私有地址（RPA）是一个使用身份解析密钥（IRK）生成的随机地址。该地址对外部观察者来说是完全随机的，因此设备可以周期性地重新生成其RPA以维护隐私，因为使用相同IRK生成的任何两个不同RPA之间没有相关性。

另一方面，IRK也可用于解析RPA，即可以检查是否使用了此IRK生成此RPA。这个过程称为“解析设备的身份”。拥有设备IRK的人总是可以尝试针对RPA解析其身份。

例如，假设设备A经常使用IRKA更改其RPA。在某些时候，A与B绑定。当A具有不同的地址时，A必须给B一种在后续连接中识别它的方法。为实现此目的，A在配对过程的密钥分发阶段分发IRKA。B将从A哪里收到的IRKA存储下来。

之后，B使用RPAX连接到的设备X。此地址看起来完全随机，但B可以尝试使用IRKA解析RPAX。如果解析操作成功，则意味着IRKA用于生成RPAX。由于IRKA属于设备A，这意味着X是A。所以B能够识别设备X的身份，但没有其他人可以做到这一点因为他们没有IRKA。

#### 4.4.1.2 非可解析私有地址

非可解析私有地址（NRPA）是一个完全随机的地址，没有生成模式，因此不能由对端解析。

无法跟踪使用经常更改的NRPA的设备，因为每个新的地址看起来都属于新的设备。

#### 4.4.1.3 多重身份解析密钥

如果一个设备与多个对端绑定，所有对端都使用RPA，则需要存储每个对端的IRK，以便以后能够识别它们（参见之前的章节）。

这意味着每当设备连接到使用未知RPA的对端设备时，它都需要尝试使用每个已存储的IRK来解析RPA。如果IRK的数量很大，那么这会引入大量的计算。

在主机中执行所有这些解析操作可能成本很高。利用硬件加速并启用控制器隐私效率会更高。

### 4.4.2 主机隐私

要启用或禁用主机隐私，可以使用以下API：
```c
bleResult_t Gap_EnableHostPrivacy
(
    bool_t       enable,
    uint8_t *    aIrk
);
```

当 enable 设置为TRUE时，aIrk 参数定义要生成的私有地址类型。如果 aIrk 为NULL，则定期生成新的NRPA并将其写入控制器。否则，IRK将从 aIrk 地址内部复制 ，并且它用于定期生成新的RPA。

私有地址（NRPA或RPA）的生命周期是 gGapHostPrivacyTimeout 外部常量所包含的秒数，该常量在 **ble_config.c** 源文件中定义。默认值为900（15分钟）。

### 4.4.3 控制器隐私

要启用或禁用控制器隐私，可以使用以下API：
```c
bleResult_t Gap_EnableControllerPrivacy
(
    bool_t                       enable,
    uint8_t *                    aOwnIrk,
    uint8_t                      peerIdCount,
    gapIdentityInformation_t*    aPeerIdentities
);
```

当 enable 设置为TRUE时，aOwnIrk 参数不应为NULL，peerIdCount 不应为0或大于 gcGapControllerResolvingListSize_c，且 aPeerIdentities 不应为NULL。

控制器使用 aOwnIrk 定义的IRK定期生成新的RPA。RPA的生命周期是 gGapControllerPrivacyTimeout 外部常量所包含的秒数，该常量在 **ble_config.c** 源文件中定义。默认值为900（15分钟）。

aPeerIdentities 是每个绑定设备的身份信息的数组。身份信息包含设备的身份地址（公共或随机静态地址）和设备的IRK。此数组可以通过 Gap_GetBondedDevicesIdentityInformation 从主机获取 。

启用控制器隐私涉及到控制器的一个快速的命令序列。序列完成后，将触发 gControllerPrivacyStateChanged_c 通用事件。

#### 4.4.3.1 扫描和启动

当中央设备正在扫描时（控制器隐私已启用），控制器会主动尝试解析广告包的广告地址字段中包含的任何PRA。如果找到针对​​对端IRK列表的任何匹配，则已扫描设备结构的 advertisingAddressResolved 参数等于TRUE。

在这种情况下，addressType 和 aAddress 字段不再包含通过OTA看到的实际广告地址，而是包含IRK能够解析广告地址的设备的身份地址。为了连接到该设备，这些字段应用于完成连接请求参数结构的 peerAddressType 和 peerAddress 字段，并且 usePeerIdentityAddress 字段应设置为TRUE。

如果 advertisingAddressResolved 等于FALSE，则广告者使用的是公共或随机静态地址，一个NRPA或一个无法解析的PRA。因此，通过将 usePeerIdentityAddress 设置为FALSE ，启动与此设备的连接，就像未启用控制器隐私一样。

> PS：蓝牙地址分为公共（public）地址和随机（random）地址。其中公共地址是固定的，由厂商设定；随机地址分为静态（static）地址和私有（private）地址，静态地址在蓝牙设备在每次上电后初始化，私有地址分为[可解析私有地址](#4411-%E5%8F%AF%E8%A7%A3%E6%9E%90%E7%A7%81%E6%9C%89%E5%9C%B0%E5%9D%80)和[非可解析私有地址](#4412-%E9%9D%9E%E5%8F%AF%E8%A7%A3%E6%9E%90%E7%A7%81%E6%9C%89%E5%9C%B0%E5%9D%80)。

#### 4.4.3.2 广告

当外设启动广告时（控制器隐私已启用），广告参数结构的 ownAddressType 字段未使用。相反，控制器总是生成一个RPA并将其作为广告地址进行广告。

#### 4.4.3.3 连接

当设备连接时（控制器隐私已启用），gConnEvtConnected_c 连接事件参数结构包含的相关字段比没有控制器隐私的要多。

如果对端使用RPA是可以通过使用从列表中IRK进行解析的，那么 peerRpaResolved 字段等于TRUE。在这种情况下， peerAddressType 和 peerAddress 字段包含已解析设备的身份地址，peerRpa 字段包含用于创建连接的实际RPA（中央在启动连接时使用的RPA，或外设广告使用的RPA）。

如果本地控制器已自动生成一个RPA（当连接已创建时），那么 localRpaUsed 字段等于TRUE，并且 localRpa 字段包含实际RPA。

