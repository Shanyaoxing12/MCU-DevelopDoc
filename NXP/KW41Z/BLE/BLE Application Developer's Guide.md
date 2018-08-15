# **Bluetooth® Low Energy Application Developer’s Guide**

- [**Bluetooth® Low Energy Application Developer’s Guide**](#bluetooth%C2%AE-low-energy-application-developer%E2%80%99s-guide)
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


# 1. 前言

本文档介绍了如何在BLE应用程序中集成Bluetooth®LowEnergy（BLE）主机栈，并提供了最常用的API和代码示例的详细说明。

该文档还列出了BLE主机栈的先决条件和初始化，然后按照层和应用程序角色分组表示API，如下所述。

首先，通用访问配置文件（GAP）层根据设备的GAP角色分为两个部分：中央和外围设备。

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

![BLE Host Stack overview](../Pic/BLE%20Application%20Developer's%20Guide-Figure1.jpg)

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

本文档中引用的所有API均可在中央和外围设备库中获得。例如，**ble_host_lib.a** 是一个功能齐全的库，完整地支持GAP级别的中央和外围设备API，以及GATT级别的客户端和服务器API。

但是，某些应用程序可能针对内存受限的设备，并不需要完整的支持。为了减少代码大小和RAM利用率，还提供了另外两个库：
* **ble_host_peripheral_lib.a**
    * 只支持用于GAP外围设备和GAP广播者角色的API
    * 只支持用于GATT服务器角色的API
* **ble_host_peripheral_lib.a**
    * 只支持用于GAP中央和GAP订阅者角色的API
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

