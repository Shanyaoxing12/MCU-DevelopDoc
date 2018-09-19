# **ZigBee 3.0 Devices User Guide** <!-- omit in toc -->

- [**Acronyms and Abbreviations**](#acronyms-and-abbreviations)
- [**1. Introduction**](#1-introduction)
    - [**1.1 ZigBee Device Types**](#11-zigbee-device-types)
    - [**1.2 Software Architecture**](#12-software-architecture)
    - [**1.3 Shared Device Structure**](#13-shared-device-structure)
    - [**1.4 Device Initialisation**](#14-device-initialisation)
    - [**1.5 Endpoint Callback Functions**](#15-endpoint-callback-functions)
    - [**1.6 Compile-Time Options**](#16-compile-time-options)
        - [**Number of Endpoints**](#number-of-endpoints)
        - [**Manufacturer Code**](#manufacturer-code)
        - [**Enabled Clusters**](#enabled-clusters)
        - [**Server and Client Options**](#server-and-client-options)
        - [**Support for Attribute Read/Write**](#support-for-attribute-readwrite)
        - [**Optional Attributes**](#optional-attributes)
- [**2. ZigBee Base Device**](#2-zigbee-base-device)
    - [**2.1 Initialising and Starting the ZigBee Base Device**](#21-initialising-and-starting-the-zigbee-base-device)
        - [**If the node was not in a network**](#if-the-node-was-not-in-a-network)
        - [**If the node was in a network**](#if-the-node-was-in-a-network)
    - [**2.2 Network Commissioning**](#22-network-commissioning)
        - [**2.2.1 Touchlink**](#221-touchlink)
        - [**2.2.2 Network Steering**](#222-network-steering)
            - [**Node is already in a network**](#node-is-already-in-a-network)
            - [**Node is not in a network**](#node-is-not-in-a-network)
        - [**2.2.3 Network Formation**](#223-network-formation)
        - [**2.2.4 Finding and Binding**](#224-finding-and-binding)
            - [**2.2.4.1 Initiator Node**](#2241-initiator-node)
            - [**2.2.4.2 Target Node**](#2242-target-node)
        - [**2.2.5 Out-Of-Band Commissioning**](#225-out-of-band-commissioning)
    - [**2.3 Network Security**](#23-network-security)
        - [**2.3.1 Centralised Security Networks**](#231-centralised-security-networks)
            - [**Install Codes**](#install-codes)
        - [**2.3.2 Distributed Security Networks**](#232-distributed-security-networks)
    - [**2.4 ZigBee Base Device Rejoin Handling**](#24-zigbee-base-device-rejoin-handling)
    - [**2.5 Attributes and Constants**](#25-attributes-and-constants)
        - [**2.5.1 Attributes**](#251-attributes)
            - [**u16bdbCommissioningGroupID**](#u16bdbcommissioninggroupid)
            - [**u8bdbCommissioningMode**](#u8bdbcommissioningmode)
            - [**ebdbCommissioningStatus**](#ebdbcommissioningstatus)
            - [**u64bdbJoiningNodeEui64**](#u64bdbjoiningnodeeui64)
            - [**au8bdbJoiningNodeNewTCLinkKey**](#au8bdbjoiningnodenewtclinkkey)
            - [**bbdbJoinUsesInstallCodeKey**](#bbdbjoinusesinstallcodekey)
            - [**u8bdbNodeCommissioningCapability**](#u8bdbnodecommissioningcapability)
            - [**bbdbNodeIsOnANetwork**](#bbdbnodeisonanetwork)
            - [**u8bdbNodeJoinLinkKeyType**](#u8bdbnodejoinlinkkeytype)
            - [**u32bdbPrimaryChannelSet**](#u32bdbprimarychannelset)
            - [**u8bdbScanDuration**](#u8bdbscanduration)
            - [**u32bdbSecondaryChannelSet**](#u32bdbsecondarychannelset)
            - [**u8bdbTCLinkKeyExchangeAttempts**](#u8bdbtclinkkeyexchangeattempts)
            - [**u8bdbTCLinkKeyExchangeAttemptsMax**](#u8bdbtclinkkeyexchangeattemptsmax)
            - [**u8bdbTCLinkKeyExchangeMethod**](#u8bdbtclinkkeyexchangemethod)
            - [**u8bdbTrustCenterNodeJoinTimeout**](#u8bdbtrustcenternodejointimeout)
            - [**bbdbTrustCenterRequireKeyExchange**](#bbdbtrustcenterrequirekeyexchange)
            - [**bTLStealNotAllowed**](#btlstealnotallowed)
            - [**bLeaveRequested**](#bleaverequested)
        - [**2.5.2 Constants**](#252-constants)
            - [**2.5.2.1 General Constants**](#2521-general-constants)
                - [**bdbcMaxSameNetworkRetryAttempts**](#bdbcmaxsamenetworkretryattempts)
                - [**bdbcMinCommissioningTime**](#bdbcmincommissioningtime)
                - [**bdbcRecSameNetworkRetryAttempts**](#bdbcrecsamenetworkretryattempts)
                - [**bdbcTCLinkKeyExchangeTimeout**](#bdbctclinkkeyexchangetimeout)
            - [**2.5.2.2 Touchlink Constants**](#2522-touchlink-constants)
                - [**bdbcTLInterPANTransIdLifetime**](#bdbctlinterpantransidlifetime)
                - [**bdbcTLMinStartupDelayTime**](#bdbctlminstartupdelaytime)
                - [**bdbcTLPrimaryChannelSet**](#bdbctlprimarychannelset)
                - [**bdbcTLRxWindowDuration**](#bdbctlrxwindowduration)
                - [**bdbcTLScanTimeBaseDuration**](#bdbctlscantimebaseduration)
                - [**bdbcTLSecondaryChannelSet**](#bdbctlsecondarychannelset)
    - [**2.6 Functions**](#26-functions)
        - [**BDB_vInit**](#bdb_vinit)
        - [**BDB_vSetKeys**](#bdb_vsetkeys)
        - [**BDB_vStart**](#bdb_vstart)
        - [**BDB_eNfStartNwkFormation**](#bdb_enfstartnwkformation)
        - [**BDB_eNsStartNwkSteering**](#bdb_ensstartnwksteering)
        - [**BDB_eFbTriggerAsInitiator**](#bdb_efbtriggerasinitiator)
        - [**BDB_vFbExitAsInitiator**](#bdb_vfbexitasinitiator)
        - [**BDB_eFbTriggerAsTarget**](#bdb_efbtriggerastarget)
        - [**BDB_vFbExitAsTarget**](#bdb_vfbexitastarget)
        - [**BDB_bIsBaseIdle**](#bdb_bisbaseidle)
        - [**BDB_u8OutOfBandCommissionStartDevice**](#bdb_u8outofbandcommissionstartdevice)
        - [**BDB_vOutOfBandCommissionGetData**](#bdb_voutofbandcommissiongetdata)
        - [**BDB_eOutOfBandCommissionGetDataEncrypted**](#bdb_eoutofbandcommissiongetdataencrypted)
        - [**BDB_bOutOfBandCommissionGetKey**](#bdb_boutofbandcommissiongetkey)
    - [**2.7 Structures**](#27-structures)
        - [**2.7.1 BDB_tsBdbEvent**](#271-bdb_tsbdbevent)
        - [**2.7.2 BDB_tuBdbEventData**](#272-bdb_tubdbeventdata)
        - [**2.7.3 BDB_tsZpsAfEvent**](#273-bdb_tszpsafevent)
        - [**2.7.4 BDB_tsFindAndBindEvent**](#274-bdb_tsfindandbindevent)
        - [**2.7.5 BDB_tsOobWriteDataToCommission**](#275-bdb_tsoobwritedatatocommission)
        - [**2.7.6 BDB_tsOobReadDataToAuthenticate**](#276-bdb_tsoobreaddatatoauthenticate)
        - [**2.7.7 BDB_tsOobWriteDataToAuthenticate**](#277-bdb_tsoobwritedatatoauthenticate)
    - [**2.8 Enumerations**](#28-enumerations)
        - [**2.8.1 BDB_teStatus**](#281-bdb_testatus)
        - [**2.8.2 BDB_teCommissioningStatus**](#282-bdb_tecommissioningstatus)
    - [**2.9 Events**](#29-events)
        - [**BDB_EVENT_ZPSAF**](#bdb_event_zpsaf)
        - [**BDB_EVENT_INIT_SUCCESS**](#bdb_event_init_success)
        - [**BDB_EVENT_REJOIN_SUCCESS**](#bdb_event_rejoin_success)
        - [**BDB_EVENT_REJOIN_FAILURE**](#bdb_event_rejoin_failure)
        - [**BDB_EVENT_NWK_STEERING_SUCCESS**](#bdb_event_nwk_steering_success)
        - [**BDB_EVENT_NO_NETWORK**](#bdb_event_no_network)
        - [**BDB_EVENT_NWK_JOIN_SUCCESS**](#bdb_event_nwk_join_success)
        - [**BDB_EVENT_NWK_JOIN_FAILURE**](#bdb_event_nwk_join_failure)
        - [**BDB_EVENT_APP_START_POLLING**](#bdb_event_app_start_polling)
        - [**BDB_EVENT_NWK_FORMATION_SUCCESS**](#bdb_event_nwk_formation_success)
        - [**BDB_EVENT_NWK_FORMATION_FAILURE**](#bdb_event_nwk_formation_failure)
        - [**BDB_EVENT_FB_HANDLE_SIMPLE_DESC_RESP_OF_TARGET**](#bdb_event_fb_handle_simple_desc_resp_of_target)
        - [**BDB_EVENT_FB_CHECK_BEFORE_BINDING_CLUSTER_FOR_TARGET**](#bdb_event_fb_check_before_binding_cluster_for_target)
        - [**BDB_EVENT_FB_CLUSTER_BIND_CREATED_FOR_TARGET**](#bdb_event_fb_cluster_bind_created_for_target)
        - [**BDB_EVENT_FB_BIND_CREATED_FOR_TARGET**](#bdb_event_fb_bind_created_for_target)
        - [**BDB_EVENT_FB_GROUP_ADDED_TO_TARGET**](#bdb_event_fb_group_added_to_target)
        - [**BDB_EVENT_FB_ERR_BINDING_FAILED**](#bdb_event_fb_err_binding_failed)
        - [**BDB_EVENT_FB_ERR_BINDING_TABLE_FULL**](#bdb_event_fb_err_binding_table_full)
        - [**BDB_EVENT_FB_ERR_GROUPING_FAILED**](#bdb_event_fb_err_grouping_failed)
        - [**BDB_EVENT_FB_NO_QUERY_RESPONSE**](#bdb_event_fb_no_query_response)
        - [**BDB_EVENT_FB_TIMEOUT**](#bdb_event_fb_timeout)
        - [**BDB_EVENT_FB_OVER_AT_TARGET**](#bdb_event_fb_over_at_target)
        - [**BDB_EVENT_LEAVE_WITHOUT_REJOIN**](#bdb_event_leave_without_rejoin)
    - [**2.10 Compile-time Options**](#210-compile-time-options)
        - [**Attributes**](#attributes)
        - [**Constants**](#constants)


# **Acronyms and Abbreviations**

| 术语 | 描述 |
| :--: | :-- |
| ACE | 辅助控制设备 |
| APDU | 应用程序协议数据单元 |
| API | 应用程序编程接口 |
| BDB | 基础设备行为 |
| CIE | 控制和指示设备 |
| DRLC | 需求响应和负荷控制 |
| HA | 家庭自动化 |
| IAS | 入侵报警系统 |
| SDK | 软件开发者工具包 |
| SE | 智能能源 |
| WD | 报警装置 |
| ZBD | ZigBee 基础设备 |
| ZCL | ZigBee 簇库 |
| ZLO | ZIgBee 照明和感应 |
| ZPS | ZigBee PRO Stack |
| ZDO | ZigBee 设备对象 |

---

PS：下文中一些常见词所对应的翻译

| 译词 | 原词 |
| :--: | :--: |
| 簇 | clusters |
| 端点 | endpoint |
| 属性 | attribute |
| 指令 | commands |
| 常量 | constant |

---

# **1. Introduction**

ZigBee无线网络的节点基于ZigBee联盟定义的设备类型，其中设备类型是确定节点支持功能的软件实体。本章介绍了ZigBee的设备类型，并叙述了ZigBee节点软件应用编程所需的相关概念。

> Note：ZigBee的设备类型在之前都是和market-specific application profiles集成在一起的，例如Home Automation。ZigBee 3.0允许不同market sectors的设备存在于同一个网路中。因此，应用程序profiles在ZigBee 3.0中并不普遍，但仍然支持向后兼容。


## **1.1 ZigBee Device Types**

设备类型是定义ZigBee节点功能的软件实体。设备类型定义一个构成此功能的簇的集合。因此簇是设备功能的基本构造块。有些簇是强制性的，有些是可选的。例如：恒温器设备使用 Basic clusters 和 Temperature Measurement clusters，也可以使用一个或多个可选的簇。

> Note：设备类型使用的簇由ZCL提供。ZCL在 ZigBee Cluster Library User Guide (ZB3ZCLUG) 中详细介绍。

设备是设备类型的实例。

一个网络节点可以支持多种设备类型。设备类型的应用程序运行在称为端点的软件实体上，每个节点最多可以有240个端点，从编号为1的端点开始。

此外，每个ZigBee 3.0节点必须支持以下的设备类型：
* ZBD：这是一个标准的设备类型，其处理一些基本的操作，例如：commissioning。该设备类型不需要端点。在第二章将详细介绍ZBD
* ZDO：这表现ZigBee节点类型（Co-ordinator、Router 或 End Device）并且具有多个通信角色（roles），该设备类型占用端点0

节 1.2 介绍了不同设备类型的相对位置（ps：在网络模型中的位置）。


## **1.2 Software Architecture**

ZigBee 3.0基本的软件架构见图Figure 1，其说明了ZigBee设备类型的位置。

![Figure 1: Basic Software Architecture](../Pic/ZigBee%203.0%20Devices%20User%20Guide-Figure1.jpg)

有关软件架构的详细信息，请参阅 ZigBee 3.0 Stack User Guide (ZB3STAUG)。


## **1.3 Shared Device Structure**

ZigBee 3.0网络中的基础操作是读取和设置相关设备的簇的属性值。在每个设备中，通过shared structure的方法在应用程序与ZCL之间交换属性值。该structure受互斥锁（mutex）的保护（在 ZCL User Guide (ZB3ZCLUG) 中描述）。特定设备的structure包含了该设备支持的簇的structures。

> Note：为了使用设备支持的簇，必须在构建时指定簇相关的选项，请参见节 1.6。

可以使用以下任意一种方法来使用shared device structure：
* 本地应用程序将属性值写入到structure中，允许ZCL响应这些属性相关的指令。
* ZCL parses（解析）传入的指令，该指令将属性值写入到structure中。然后写入的值可以被本地应用程序读取。

涉及shared device structure的远程读写操作如图 Figure 2 所示。有关这些操作的详细描述，请参阅 ZCL User Guide (ZB3ZCLUG)。

> Note：shared device structure位于server设备（which hosts the cluster server to be accessed）上。client设备（which performs the remote access, hosts the corresponding cluster client）。

![Figure 2: Operations using Shared Device Structure](../Pic/ZigBee%203.0%20Devices%20User%20Guide-Figure2.jpg)

> Note：如果没有远程属性写入，则设备上的簇server（in the shared structure）的属性通过本地应用程序进行维护。


## **1.4 Device Initialisation**

ZigBee 3.0应用程序的初始化如 ZigBee 3.0 Stack User Guide (ZB3STAUG) 的 “Forming and Joining a Network” 部分所述。此外，必须执行一些设备的初始化。

ZigBee设备的初始化必须根据下面的顺序执行：

1. 在头文件 **zcl\_options.h** 中，启用所需的编译时选项。这些选项包括：设备要使用的簇、每个簇的 client/server 状态和每个簇的可选属性。有关编译时选项的信息，请参阅第 1.6 节。
2. 在应用程序中，通过声明文件范围变量来创建设备structure的示例。例如：
    ```c
    tsZLO_DimmableLightDevice sDevice;
    ```

3. 在应用程序的初始化部分，通过你的代码设置设备的handled，如下所示：
    1. 设置设备要使用的簇属性的初始值，例如：
        ```c
        sDevice.sBasicCluster.u8StackVersion = 1;
        sDevice.sBasicCluster....
        ```
        
    2. 在调用 ZPS\_eAplAfInit() 之前和在调用 eZCL\_Initialise() 之后，通过调用相关的设备注册函数来注册设备。例如：eZLO\_RegisterDimmableLightEndPoint()。在这个函数调用中，设备必须分配一个端点（在1 - 240的范围中）。另外，必须指定设备structure以及用户定义的回调函数，该函数将在与端点相关的event发生时被调用（参阅第 1.5 节）。一旦该函数被调用，shared device structure可以被另一个设备读取。
    3. 在调用 ZPS\_eAplAfInit() 之后，通过先调用 BDB\_vInit() 然后调用 BDB\_vStart() 来初始化并启动ZBD（请参阅第 2.1 节以了解 ZBD 初始化的更多细节）。

> Note 1：设备类型说明中详细介绍了不同设备类型的端点注册函数集。例如，第 3 章 Lighting and Occupancy devices。

> Note 2：设备注册函数创建设备使用的所有簇的实例，因此不需要明确地调用各个簇的创建函数，例如：创建 Identify cluster 的 eCLD\_IdentifyCreateIdentify()。


## **1.5 Endpoint Callback Functions**

必须为使用的每个端点提供用户定义的回调函数，当发生与端点有关的event（例如 incoming 消息）时，将调用此函数。当设备类型（端点支持）的端点使用注册函数注册时，其回调函数会被注册。例如，一个 On/Off Light 设备使用 eZLO\_RegisterOnOffLightEndPoint() 函数（参见第 3.1 节）。端点回调函数的类型定义如下：
```c
typedef void (* tfpZCL_ZCLCallBackFunction) (tsZCL_CallBackEvent *pCallBackEvent);
```

其中 pCallBackEvent 是event的指针。

> Note：没有关联端点的events通过 general stack-supplied 回调函数 APP\_vGenCallback() 传递。例如，应用程序可以通过调用此回调函数接收 stack leave and join events。Stack events 在 ZigBee 3.0 Stack User Guide (ZB3STAUG) 中介绍。


## **1.6 Compile-Time Options**

在ZigBee 3.0应用程序编译之前，必须在应用程序的头文件 **zcl\_options.h** 中配置编译时选项。

> Note 1：Cluster-specific的编译时选项在 ZCL User Guide (ZB3ZCLUG) 的簇说明中详细描述。

> Note 2：此外，ZBD的编译时选项必须在文件 **bdb\_options.h** 中设置，参见第 2.10 节。


### **Number of Endpoints**

必须指定应用程序使用的最高端点编号，如：
```c
#define BDB_FB_NUMBER_OF_ENDPOINTS 3
```

通常，应用程序从端点1开始使用，因此上述例子中，将使用端点1到3。然而，可以使用编号较低的端点作非应用程序用途，即在端点1和2上运行其他协议，并在端点3上运行应用程序。在设置 BDB\_FB\_NUMBER\_OF\_ENDPOINTS 为3时，一些存储空间将会被静态分配给端点1和2，但其未被使用。需要注意，此定义仅适用于本地端点，应用程序可以引用远程端点（其编号值超出所定义的 BDB\_FB\_NUMBER\_OF\_ENDPOINTS 值）。


### **Manufacturer Code**

ZCL允许一些开发设备的制造商定义制造商码。这是由ZigBee联盟分配给制造商的16-bit值，设置如下：
```c
#define ZCL_MANUFACTURER_CODE 0x1037
```

以上示例将制造商码的默认值设置为 0x1037（which belongs to NXP），但制造商（ps：指使用其芯片开发的厂商）应该设置属于自己的分配值。


### **Enabled Clusters**

所有需要的簇必须在头文件的选项中启用。例如，一个 On/Off Light 设备的应用程序可能需要使用以下的簇定义：
```c
#define CLD_BASIC
#define CLD_IDENTIFY
#define CLD_GROUPS
#define CLD_SCENES
#define CLD_ONOFF
```


### **Server and Client Options**

许多簇都具有表明本地设备上的簇是server行为还是client行为的选项。如果上述定义之一的簇被启用，则必须定义簇的 server/client 状态。例如，使用 Groups cluster 作为server，需在头文件中包含以下定义：
```c
#define GROUPS_SERVER
```


### **Support for Attribute Read/Write**

对簇属性的 读/写 访问必须显式编译到应用程序中，并且必须在选项头文件中使用以下的宏，以单独地使能一个簇的server和client方：
```c
#define ZCL_ATTRIBUTE_READ_SERVER_SUPPORTED
#define ZCL_ATTRIBUTE_READ_CLIENT_SUPPORTED
#define ZCL_ATTRIBUTE_WRITE_SERVER_SUPPORTED
#define ZCL_ATTRIBUTE_WRITE_CLIENT_SUPPORTED
```

注意，上述的每个定义将在应用程序中应用到所有簇上。


### **Optional Attributes**

许多簇具有可选属性，可以在编译时通过选项头文件将其启用。例如，如下启用了 Basic cluster 中的 “application version” 属性：
```c
#define CLD_BAS_ATTR_APPLICATION_VERSION
```

---

# **2. ZigBee Base Device**

ZBD是ZigBee 3.0网络上的所有节点都必须具有的设备类型，它和节点上的一个或多个其他的设备类型并存，但它不需要端点。ZBD提供一个框架，以使用ZigBee的设备类型。它执行所有节点可能需要的基本功能，确保所有节点的行为一致，特别是关于网络的创建和加入，以及网络安全。

本章介绍ZBD的network commissioning和安全功能，以及在KW41Z的ZigBee 3.0应用程序中执行这些功能所需的NXP资源。有关ZBD的详细信息，请参阅ZigBee联盟提供的 ZigBee Base Device Behavior Specification (13-0402)。


## **2.1 Initialising and Starting the ZigBee Base Device**

必须在应用程序代码中使用函数 BDB\_vInit() 以初始化ZBD。在ZigBee PRO stack初始化以及从persistent storage中恢复ZBD的属性（bbdbNodeIsOnANetwork）之后，必须调用此函数。

> Note 1：BDB\_vInit() 的内部调用了函数 BDB\_vSetKeys()，该函数从文件 **bdb\_link\_keys.c** 中将预先配置的链接密钥加载到内存中。网络安全和预配置链接密钥在 2.3 节中详述。

> Note 2：ZBD需要多个内部软定时器，由宏 BDB\_ZTIMER\_STORAGE 定义。因此，当应用程序调用 ZTIMER\_eInit() 以初始化所需的软定时器并为其分配存储空间（数组元素）时，它必须添加 BDB\_ZTIMER\_STORAGE 的定时器以供ZBD使用。该函数必须在 BDB\_vInit() 之前调用。软定时器及其相关功能在 ZigBee 3.0 Stack User Guide (ZB3STAUG) 中详述。

然后，可以通过调用 BDB\_vStart() 来启动ZBD。根据节点的类型以及其之前是否为网络的成员，此函数可能执行或不执行该动作，如下所述。无论是那种情况，该函数最终都会使用合适的event调用回调函数 APP\_vBdbCallback()。


### **If the node was not in a network**

对于支持Touchlink commissioning的Router节点（参见 2.2.1 节），此函数会在 BDBC\_TL\_PRIMARY\_CHANNEL\_SET 的 bitmap（参见 2.5.2.2 节）中为节点从Touchlink指定的主要信道集合中选择一个无线信道。其会选择指定集合中的第一个信道，或者如果宏 RAND\_CHANNEL 被设置为TRUE（在文件 **bdb\_options.h** 中）时，将随机从集合中选择一个信道。

对于Co-ordinator、其他Router和End Device节点将不采取任何行动，并且应用程序随后需要使用在 2.2 节中描述的一种commissioning方法形成网络（Co-ordinator or Router）或加入网络（End Device or Router）。

以上的情况，函数会生成一个 BDB\_EVENT\_INIT\_SUCCESS event。


### **If the node was in a network**

对于Co-ordinator和Router节点，不采取任何行动，并且函数会生成一个 BDB\_EVENT\_INIT\_SUCCESS event。

对于End Device节点，此函数会尝试让节点重连到网络中。它会执行一系列的周期重连，每个周期包含以下三次重连尝试：
1. 使用之前的网络参数（without network discovery）进行尝试
2. 在 u32bdbPrimaryChannelSet 的 bitmap（属性）中指定的主要信道集上进行network discovery并尝试
3. 在 u32bdbPrimaryChannelSet 的 bitmap（属性）中指定的次要信道集上进行network discovery并尝试

信道的 bitmap 是ZBD的属性，在 2.5.1 节中详述。

上述的周期重连会被执行最多 BDBC\_IMP\_MAX\_REJOIN\_CYCLES 次，这是一个特定实现的（implementation-specific）ZBD常量（参见 2.5.2 节）。

如果尝试重连成功，函数将会生成 BDB\_EVENT\_REJOIN\_SUCCESS event。

如果所有的尝试重连都不成功，函数将会生成 BDB\_EVENT\_REJOIN\_FAILURE event，除非通过APS的 apsUseInsecureJoin 属性启用了unsecured joins，在这种情况下，该函数将尝试通过Network Steering（在 2.2.2 节中描述）进行加入。加入的性质取决于APS属性 ApsUseExtendedPanid 中设置的Extended PAN ID（EPID）的值：
* 对于non-zero EPID，该节点会尝试加入这个EPID相关的网络
* 对于zero EPID，函数会尝试加入到可用的网络

此连接将尝试自动调用函数 BDB\_eNsStartNwkSteering()。


## **2.2 Network Commissioning**

Network commissioning包括下列的活动：
* 创建一个网络
* 允许设备加入到网络中（通过本地节点）
* 加入一个网络
* 将一个本地端点与远程节点的一个端点进行Binding
* 往group中添加一个远程节点

可以由单个节点执行commissioning activities（取决于ZigBee节点的类型：Co-ordinator, Router, End Device），以及为节点启用commissioning modes。通过ZBD可以使用多种不同的commissioning modes。下表中列出这些modes及其支持的commissioning activities。

| Commissioning Mode | 功能 |
| :-- | :-- |
| Touchlink | 创建一个新网络；允许其他设备加入现有的网络；将本地设备连接到现有的网络 |
| Network Steering | 允许其他设备加入现有的网络；将本地设备连接到现有的网络 |
| Network Formation | 创建一个新的网络 |
| Finding and Binding | 将一个本地的端点与远程节点的一个端点进行 Binding；往 group 中添加一个远程节点 |

commissioning modes通过属性 u8bdbCommissioningMode 进行单独的 启用/禁止，如下表所示。该属性是一个 bitmap，每个mode都有一个位（对应位置为 ‘1’ 为启用，置为 ‘0’ 为禁止）。Enumerations可以用于启用各个mode（将其位置 ‘1’）。

| Bit | Commissioning Mode | Enumeration |
| :--: | :-- | :-- |
| 0 | Touchlink	| BDB\_COMMISSIONING\_MODE\_TOUCHLINK |
| 1 | Network Steering | BDB\_COMMISSIONING\_MODE\_NWK\_STEERING |
| 2	| Network Formation | BDB\_COMMISSIONING\_MODE\_NWK\_FORMATION |
| 3	| Finding and Binding | BDB\_COMMISSIONING\_MODE\_FINDING\_N\_BINDING |

节点上的当前commissioning状态反映在属性 ebdbCommissioningStatus 上。

在NXP的ZBD实现中，在应用程序的控制下使用提供的API函数启动各个commissioning modes。如果要启用一个mode并且节点类型与该mode是有关的（例如，End Device不能执行Network Formation），则应用程序可以调用commissioning mode。

commissioning modes在下面的小节中概述。要了解关于这些modes的更多信息，请参阅 ZigBee Base Device Behavior Specification(13-0402-08)。

> Note：节点通常通过用户的动作来提示enter commissioning，例如按下节点上的按键。此操作可能代表节点的全部或单个端点。


### **2.2.1 Touchlink**
 
Touchlink commissioning可以用于形成一个新的网络 and/or 将节点加入到一个现有的网络中。Touchlink是由一个称为 “initiator” 的节点发起的，它可能是现有网络中的一个成员，或者（if not）是将创建新网络的节点。在这两种情况下，initiator都会将第二个节点（称为 “target” 节点）加入到网络中。

Touchlink在ZCL中作为簇来提供。initiator必须支持 Touchlink cluster 作为client，目标节点必须支持该簇作为server。如果这是节点需要的，则必须通过ZBD属性 u8bdbCommissioningMode 来启用Touchlink commissioning。有关 Touchlink Commissioning cluster 及其实现，请参阅 ZigBee Cluster Library User Guide(ZB3ZCLUG)。

可以提供 “Touchlink 预配置链接密钥”，在节点commissioning到安全的网络的期间使用（见 2.3 节）。

如果Touchlink commissioning不成功，则通过属性 ebdbCommissioningStatus 指示状态 NO\_SCAN\_RESPONSE （所有其他状态表示成功）。


### **2.2.2 Network Steering**

Network Steering可以将本地节点加入到现有的网络，或允许其他节点通过本地节点加入到网络。

如果节点需要Network Steering，则必须通过属性 u8bdbCommissioningMode 启用。你可以在你的应用程序中通过调用函数 BDB\_eNsStartNwkSteering() 来启动Network Steering。

所采用的途径取决于本地节点是否已经是网络中的成员（通过布尔类型属性 bbdbNodeIsOnANetwork 指示）。在所有情况下，Network Steering的结果通过传入回调函数 APP\_vBdbCallback() 的event来指示。


#### **Node is already in a network**

当节点已经是网络中的成员时，它会在一个固定的时间内开放网络，通过广播一个 Management Permit Joining 请求来让其他节点加入（任何节点类型都可以以这种方式开放网络）。这个固定的时间周期默认为180秒，可以通过ZBD常量 BDBC\_MIN\_COMMISSIONING\_TIME 配置（以秒为单位），见 2.5.2 节。在发起上述广播后，将生成 BDB\_EVENT\_NWK\_STEERING\_SUCCESS event。


#### **Node is not in a network**

当节点不是网络中的成员并且是Router或End Device时，它会搜索一个合适并可以加入的网络，如果找到，则尝试加入网络，如下所示：
1. 节点通过扫描 u32bdbPrimaryChannelSet 的 bitmap（属性）指定的主要无线信道集合来执行network discovery。如果没有找到开放的网络，则通过 u32bdbSecondaryChannelSet 的 bitmap（属性）指定的次要无线信道集合来重复network discovery。如果仍然没有找到网络，则生成 BDB\_EVENT\_NO\_NETWORK event，并且放弃Network Steering。
2. 如果找到至少一个开放网络，节点将逐个尝试加入，最多尝试 BDBC\_MAX\_SAME\_NETWORK\_RETRY\_ATTEMPTS 次。如果加入成功，则属性 bbdbNodeIsOnANetwork 会被设置为TRUE。如果在主要信道扫描后加入不成功，将在次要信道上重复（步骤1）扫描。如果次要信道扫描后仍然加入不成功，则会生成 BDB\_EVENT\_NWK\_JOIN\_FAILURE event，并放弃Network Steering。
3. 已验证的加入节点从其父节点上接收网络密钥。如果加入的网络为集中式安全网络（具有Trust Centre），则节点向Trust Centre单播一个 Node Descriptor 请求。检查接收到的Node Descriptor以确保Trust Centre支持ZigBee PRO stack r21或以上版本。如果是这种情况，节点会执行检索一个新的Trust Centre链接密钥的过程以替换其预配置链接密钥。在任何时候发生失败，都会通过 BDB_EVENT_NWK_JOIN_FAILURE event 向应用程序指示。
4. 在成功完成上述步骤后，加入节点请求的 “permit joining” 时间（用于新节点加入网络）通过 BDBC\_MIN\_COMMISSIONING\_TIME（默认为180秒）延长，并且为应用程序生成一个 BDB\_EVENT\_NWK\_STEERING\_SUCCESS event。

取决于上述Network Steering过程的结果：
* 如果节点成功加入网络，你可能希望将节点与另一个节点bind或添加到一个group中。在这种情况下，必须进行 2.2.4 节 所述的Finding and Binding阶段。
* 如果节点加入网络失败，你可能希望确保所需的网络是开放加入的并且重新发起此Network Steering过程。在节点是Router的情况下，应用程序可以选择形成自己的分布式网络，这种情况下，节点必须进行Network Formation阶段（在 2.2.3 节中描述）。


### **2.2.3 Network Formation**

Network Formation允许Co-ordinator或Router创建一个新的网络。
* Co-ordinator会形成一个集中式安全网络（见 2.3.1 节），并激活其Trust Centre功能
* Router会形成一个分布式安全网络（见 2.3.2 节）

如果节点需要Network Formation，则必须通过属性 u8bdbCommissioningMode 来启用它。你可以在你的应用程序中通过调用函数 BDB\_eNfStartNwkFormation() 来启动Network Formation。

节点将通过 u32bdbPrimaryChannelSet 的 bitmap（属性）指定的主要无线信道集合来执行扫描，在其中一个空闲的主要信道上形成一个具有唯一PAN ID的集中式或分布式网络。如果此网络形成失败或主要信道的 bitmap 为零，则节点会通过 u32bdbSecondaryChannelSet 的 bitmap（属性）指定的次要无线信道集合来执行扫描，在其中一个空闲的次要信道上形成一个具有唯一PAN ID的集中式或分布式网络。

在通过Router形成一个分布式安全的网络期间：
* 上述的信道扫描将从相关设置的第一个信道开始，并覆盖所有指定的信道
* 如果宏 RAND\_CHANNEL 为TRUE（在应用程序中），将从扫描的信道中随机选择一个信道
* 应该将宏 RAND\_DISTRIBUTED\_NWK\_KEY 设置为TRUE，以随机选择一个网络密钥（但可以在应用程序开发期间将其设置为FALSE，以便使用特定的网络密钥）
* PAN ID和EPID是随机分配的（但必须不能和附近其他的运行中的网络冲突）
* 本地的16-bit网络地址是随机分配的

在所有的情况下，成功的Network Formation会通过回调函数 APP\_vBdbCallback() 以 BDB\_EVENT\_NWK\_FORMATION\_SUCCESS event 来指示；如果不成功，则通过 BDB\_EVENT\_NWK\_FORMATION\_FAILURE event 来指示。

如果Network Formation成功，新网络将仅仅由一个节点组成。更多的节点可以通过使用Network Steering（见 2.2.2 节）或Touchlink（见 2.2.1 节）来添加到网络中。


### **2.2.4 Finding and Binding**

Finding and Binding mode允许网络中的一个节点与另一个网络节点配对。例如，可以将灯与控制设备配对，以允许其控制灯。此commissioning mode的目的是将一个新节点上的一个端点与网络中的一个远程设备上的一个兼容端点进行bind（取决于支持的簇）。或者，可以将新节点添加到一个集体控制的节点group中。

如果节点需要Finding and Binding，则必须通过属性 u8bdbCommissioningMode 启用。

在Finding and Binding中，一个节点可以为下列两种角色之一：
* Initiator（发起者）：此节点可以创建一个与远程端点的（本地）binding，也可以请求将远程端点添加到group中
* Target（目标）：此节点标识（identifies）自己，并接收和响应来自发起者的请求

预期的结果是发起者和目标之间配对。通常，发起者是一个控制设备。Finding and Binding过程所遵循的途径取决于本地端点是发起者还是目标。


#### **2.2.4.1 Initiator Node**

可以通过调用函数 BDB\_eFbTriggerAsInitiator() 在发起节点上启动Finding and Binding（该函数可以作为节点上的用户操作的结果而被调用，例如按下按钮）。发起者会在Finding and Binding mode中保持一个固定的时间间隔，该时间间隔由常量 BDBC\_MIN\_COMMISSIONING\_TIME 定义。如果Finding and Binding在此时间内不成功，则会生成 BDB\_EVENT\_FB\_TIMEOUT event 并传递到回调函数 APP\_vBdbCallback() 中。

一旦开始Finding and Binding，发起节点通过周期性地（以秒为单位）广播一个 Identify Query 指令来搜索目标端点，其周期通过宏 BDB\_FB\_RESEND\_IDENTIFY\_QUERY\_TIME 定义。

> Note：在每个广播尝试之前，BDB\_EVENT\_FB\_NO\_QUERY\_RESPONSE event 会被生成，并传递到 APP\_vBdbCallback() 中，以便让应用程序有机会退出当前的Finding and Binding过程（见下文）。

如果发起者接收到来自远程端点的 Identify Query 响应，则应用程序必须使用函数 BDB\_vZclEventHandler() 将 BDB\_E\_ZCL\_EVENT\_IDENTIFY\_QUERY ZCL event 传递给Base Device。这将允许Base Device通过向相关端点发送一个 Simple Descriptor 请求来收集关于标识设备的信息。如果成功接收到所请求的Simple Descriptor，则回调函数会检查这个descriptor的簇是否与发起者相匹配。通过传入 APP\_vBdbCallback() 的 BDB\_EVENT\_FB\_HANDLE\_SIMPLE\_DESC\_RES \_OF\_TARGET event 来通知应用程序。

如果至少有一个匹配的簇，发起者会执行以下的操作之一：
* 如果需要binding（通过 u16bdbCommissioningGroupID 属性等于 0xFFFF 来指示），发起者将远程端点添加到本地Binding table中（但可能首先需要请求远程节点的 IEEE/MAC 地址）。
* 如果需要grouping（通过 u16bdbCommissioningGroupID 属性等于16-bit group地址来指示），发起者将请求目标端点将group地址添加到其Group Address table中。

通过以下event来通知应用程序（是否）成功binding或grouping：
* binding：
    * BDB\_EVENT\_FB\_BIND\_CREATED\_FOR\_TARGET 为成功
    * BDB\_EVENT\_FB\_ERR\_BINDING\_FAILED 为失败
* grouping：
    * BDB\_EVENT\_FB\_GROUP\_ADDED\_TO\_TARGET 为成功
    * BDB\_EVENT\_FB\_ERR\_GROUPING\_FAILED 为失败

此时，应用程序可以通过调用 Identify cluster 函数 eCLD\_IdentifyCommandIdentifyRequestSend() 来请求将identification mode周期设置为零，以远程停止目标节点上的identification mode（因为Finding and Binding的）。

可以使用函数 BDB\_vFbExitAsInitiator() 来停止在发起端点上的Finding and Binding过程。回调函数 APP\_vBdbCallback() 典型地调用此函数，以作为用户操作的结果，例如按下按钮或释放按钮。


#### **2.2.4.2 Target Node**

可以通过调用函数 BDB\_eFbTriggerAsTarget() 在目标节点上启动Finding and Binding（该函数可以作为节点上的用户操作的结果而被调用，例如按下按钮）。

然后目标节点使用 Identify cluster 将自己在一段固定时间里置于identification mode。这个时间段（以秒为单位）由 u16IdentifyTime 决定，它是一个自动设置常量 BDBC\_MIN\_COMMISSIONING\_TIME 的值的 Identify cluster 属性。在identification mode下，该簇会响应任何接收到的 Identify Query 指令以及其他的Finding and Binding指令。该节点也可以在视觉或听觉上指示其处于identification mode下。在上述周期结束时退出identification mode，该簇将不能够处理 Identify Query 指令，但该节点仍然可以服务来自发起者的其他与 binding/grouping 相关的指令。Identify cluster 在 ZigBee Cluster Library User Guide(ZB3ZCLUG) 中有完整的描述。

目标节点可以通过以下任一方式从Finding and Binding过程中移出：
* 本地应用程序可以调用函数 BDB\_vFbExitAsTarget() 作为用户操作的结果，例如按下按钮或释放按钮。
* 远程应用程序（发起者上）可以调用 Identify cluster 函数 eCLD\_IdentifyCommandIdentifyRequestSend() 来请求将identification mode周期设置为零。为了向Base Device指示identification过程已结束，应用程序必须使用函数 BDB\_vZclEventHandler() 将 BDB\_E\_ZCL\_EVENT\_IDENTIFY ZCL event 传递给Base Device。这将允许Base Device退出目标端点上的 “Finding and Binding” 过程。


### **2.2.5 Out-Of-Band Commissioning**

一个节点可以通过out-of-band的方式commissioned到ZigBee网络。也就是说，不使用在目标网络使用的无线信道上运行的IEEE802.15.4数据包。例如，可以从另一台使用inter-PAN数据包（运行在不同的无线信道上）的ZigBee设备或使用NFC（近场通信）的commissioning设备来引导out-of-band commissioning。

通过启动Co-ordinator或将 Router/End Device 加入到现有的网络，可以使用out-of-band commissioning来创建新的网络。为此，commissioning数据必须通过out-of-band的方式发送到节点。这些数据包含了网络的细节（见 2.7.5 节）。应用程序必须将收到的commissioning数据传递给ZBD，并且使用函数 BDB\_u8OutOfBandCommissionStartDevice() 来启动out-of-band commissioning。然后这些数据将存储在本地。

作为节点到现有集中式网络的out-of-band commissioning的一部分，被加入网络的Trust Centre可能需要通过检查节点是否包含合适数据值来验证新的节点，如正确的网络密钥和Trust Centre地址。如果节点收到这样的验证请求，则应用程序可以通过两种方式来请求所需的数据值：
* 可以使用函数 BDB\_vOutOfBandCommissionGetData() 来读取相关的数据值。在这种情况下，应用程序应在将数据发送到Trust Centre之前加密获取的网络密钥。应在此加密中使用该节点的install code。
* 可以使用函数 BDB\_eOutOfBandCommissionGetDataEncrypted() 来读取相关的数据值，并对获取的网络密钥进行加密。因此，网络密钥将以已加密的方式传递。必须在此函数调用中指定要在加密中使用的此节点的install code。然后，应用程序可以将获得的数据发送到Trust Centre。

一旦Trust Centre接收到请求的数据，它可以使用函数 BDB\_bOutOfBandCommissionGetKey() 来解密获得得网络密钥，然后检查是否正在使用正确的密钥。此函数需要新节点的install code，该code必须通过out-of-band的方式（例如通过小键盘）提供给Trust Centre。

安全密钥和install code在 2.3 节中详述。


## **2.3 Network Security**

ZBD支持以下网络安全模式：
* 集中式安全网络（Centralised security）
* 分布式安全网络（Distributed security）

下面的小节将对其进行详述。

所有的Router和End Device节点都应该通过适应他们加入的网络所采用的安全方案来支持集中式安全和分布式安全。Co-ordinator只支持集中式安全。

当应用程序调用 BDB\_vInit() 时，该函数会在其内部调用函数 BDB\_vSetKeys()。此函数将根据节点类型是否支持 集中式/分布式 安全来加载合适的预配置链接密钥。预配置链接密钥在文件 **bdb\_link\_keys.c** 中定义。


### **2.3.1 Centralised Security Networks**

集中式安全网络由Co-ordinator形成，并且Co-ordinator会充当该网络的Trust Centre。当一个节点尝试加入到网络中时，Trust Centre会在允许其加入到网络前对其进行验证。

为了参与集中式安全网络，所有的节点必须具有预配置链接密钥。当Trust Centre传递网络密钥到新的节点上时，使用此密钥对网络密钥进行加密。当一个节点加入集中式安全网络时，ZBD会自动使用相关的预配置链接密钥。在Co-ordinator形成集中式安全网络时也是如此。

下列的密钥类型可以用于集中式安全的预配置：
* Default Global Trust Centre Link Key：此密钥factory-programmed到所有节点中，其用于加密Trust Centre与加入节点之间的通信。
* Touchlink Pre-configured Link Key：此密钥factory-programmed到所有节点中，其可以用于Touchlink commissioning和加密父系路由与加入节点之间的通信。其可以是下列三种类型之一（最终产品应该使用 “Master key”）：
    * Development key，在ZigBee认证之前的开发中使用
    * Master key，在成功的ZigBee认证之后使用
    * Certification key，在ZigBee认证测试期间使用
* Install Code-derived Pre-configured Link Key：此密钥由ZigBee stack从factory中分配给每个Router和End Device节点的随机install code派生而来的。install code is factory-programmed到节点中，但在节点commissioned时通过out-of-band的方式提供给Trust Centre。下面将更加详细地描述install code的使用。


#### **Install Codes**

install code可用于创建初始链接密钥，以用于将单个节点commissioning到集中式安全网络中。install code在factory中分配给节点。这是一个随机码，但不一定是唯一的（可能为多个节点随机生成同一个install code）。ZigBee stack使用Matyas-Meyer-Oseas哈希函数通过install code派生出一个链接密钥。install code is factory-programmed到节点中，并且在其离开factory时也伴随该节点（e.g. in printed form）。下面概述使用install code commission节点的过程。

在factory中：
1. 为单个节点生成一个随机的install code
2. 将install code编程到节点中
3. ZigBee stack通过install code派生一个预配置链接密钥
4. install code随节点一起提供（by some unspecified means）

安装过程中：
1. 随节点一起提供的install code安装到 Co-ordinator/Trust Centre 中
2. 预配置链接密钥由 Co-ordinator/Trust Centre 的ZigBee stack通过install code派生
3. Trust Centre和节点随后使用预配置链接密钥将节点加入到网络中（例如，用以 加密/解密 网络密钥）

更多关于install code的详细信息可以在 ZigBee Base Device Behavior Specification(13-0402-08) 中找到。


### **2.3.2 Distributed Security Networks**

分布式安全网络由Router形成，并且其没有Trust Centre，它只包含Routers和End Devices。当一个节点尝试加入到网络时，它的父系路由节点会对其进行认证，然后允许其进入网络。

为了参与分布式安全网络，所有Router和End Device节点必须预配置一个链接密钥。此密钥用于加密父系路由传递给新加入节点的网络密钥。当Router或End Device加入分布式安全网络时，ZBD将自动使用相关的预配置链接密钥。在Router形成新的分布式安全网络时也是如此。

下列的密钥类型可以用于分布式安全的预配置：
* Distributed Security Global Link Key：此密钥factory-programmed到所有节点中，其用于加密父系路由与加入节点之间的通信。
* Touchlink Pre-configured Link Key：此密钥factory-programmed到所有节点中，其可以用于Touchlink commissioning和加密父系路由与加入节点之间的通信。其可以是下列三种类型之一（最终产品应该使用 “Master key”）：
    * Development key，在ZigBee认证之前的开发中使用
    * Master key，在成功的ZigBee认证之后使用
    * Certification key，在ZigBee认证测试期间使用


## **2.4 ZigBee Base Device Rejoin Handling**

对于Router或End Device，有些情况下ZigBee PRO stack将启动网络重新加入尝试。这些包括：
* 一个Router或End Device接收到一个 “leave with rejoin” 请求
* 一个End Device轮询其父系的数据，但没有接收到响应

ZBD处理由此重新加入尝试产生的stack event：
* 如果接收到 ZPS\_EVENT\_NWK\_FAILED\_TO\_JOIN（指示重新加入不成功）stack event，则ZBD进行一系列重新加入尝试，如 2.1 节中 “If the node was in a network” 情况所述。如果重新加入尝试成功，则生成 BDB\_EVENT\_REJOIN\_SUCCESS event 以通知应用程序；如果所有的重新加入都不成功，则会生成 BDB\_EVENT\_REJOIN\_FAILURE event，除非启用了unsecured joins（尝试通过 Network Steering 进行加入）。
* 如果接收到 ZPS\_EVENT\_NWK\_JOINED\_AS\_ROUTER 或 ZPS\_EVENT\_NWK\_JOINED\_AS\_END\_DEVICE（指示重新加入成功）stack event，则生成 BDB\_EVENT\_REJOIN\_SUCCESS event 以通知应用程序。


## **2.5 Attributes and Constants**

### **2.5.1 Attributes**

ZBD的属性包含在 structure BDB\_tsAttrib 中，如下所示
```c
typedef struct
{
    uint16                       u16bdbCommissioningGroupID;
    uint8                        u8bdbCommissioningMode;
    BDB_teCommissioningStatus    ebdbCommissioningStatus;
    uint64                       u64bdbJoiningNodeEui64;
    uint8                        au8bdbJoiningNodeNewTCLinkKey[16];
    bool_t                       bbdbJoinUsesInstallCodeKey;
    const uint8                  u8bdbNodeCommissioningCapability;
    bool_t                       bbdbNodeIsOnANetwork;
    uint8                        u8bdbNodeJoinLinkKeyType;
    uint32                       u32bdbPrimaryChannelSet;
    uint8                        u8bdbScanDuration;
    uint32                       u32bdbSecondaryChannelSet;
    uint8                        u8bdbTCLinkKeyExchangeAttempts;
    uint8                        u8bdbTCLinkKeyExchangeAttemptsMax;
    uint8                        u8bdbTCLinkKeyExchangeMethod;
    uint8                        u8bdbTrustCenterNodeJoinTimeout;
    bool_t                       bbdbTrustCenterRequireKeyExchange;
    bool_t                       bTLStealNotAllowed;
    bool_t                       bLeaveRequested;
}BDB_tsAttrib;
```

ZBD的属性值可以在编译时在 **bdb\_options.h** 文件中使用下表中列出的宏进行初始化（有关编译时选项的信息，请参阅 2.10 节）。通过上述structure，可以在运行时写入或读取属性。

> Note：bTLStealNotAllowed 和 bLeaveRequested 是NXP私有变量，非ZigBee的属性。

| 属性                                | 初始化宏                                    |
| :---------------------------------- | :------------------------------------------ |
| u16bdbCommissioningGroupID          | BDB\_COMMISSIONING\_GROUP\_ID               |
| u8bdbCommissioningMode              | BDB\_COMMISSIONING\_MODE                    |
| ebdbCommissioningStatus             | BDB\_COMMISSIONING\_STATUS                  |
| u64bdbJoiningNodeEui64              | BDB\_JOINING\_NODE\_EUI64                   |
| au8bdbJoiningNodeNewTCLinkKey\[16\] | -                                           |
| bbdbJoinUsesInstallCodeKey          | BDB\_JOIN\_USES\_INSTALL\_CODE\_KEY         |
| u8bdbNodeCommissioningCapability    | -                                           |
| bbdbNodeIsOnANetwork                | -                                           |
| u8bdbNodeJoinLinkKeyType            | BDB\_NODE\_JOIN\_LINK\_KEY\_TYPE            |
| u32bdbPrimaryChannelSet             | BDB\_PRIMARY\_CHANNEL\_SET                  |
| u8bdbScanDuration                   | BDB\_SCAN\_DURATION                         |
| u32bdbSecondaryChannelSet           | BDB\_SECONDARY\_CHANNEL\_SET                |
| u8bdbTCLinkKeyExchangeAttempts      | BDB\_TC\_LINK\_KEY\_EXCHANGE\_ATTEMPTS      |
| u8bdbTCLinkKeyExchangeAttemptsMax   | BDB\_TC\_LINK\_KEY\_EXCHANGE\_ATTEMPTS\_MAX |
| u8bdbTCLinkKeyExchangeMethod        | BDB\_TC\_LINK\_KEY\_EXCHANGE\_METHOD        |
| u8bdbTrustCenterNodeJoinTimeout     | BDB\_TRUST\_CENTER\_NODE\_JOIN\_TIMEOUT     |
| bbdbTrustCenterRequireKeyExchange   | BDB\_TRUST\_CENTER\_REQUIRE\_KEYEXCHANGE    |
| bTLStealNotAllowed                  | -                                           |
| bLeaveRequested                     | -                                           |

这些属性分布在下面描述。有关更多详情，请参阅 ZigBee Base Device Behavior Specification(13-0402-08)。


#### **u16bdbCommissioningGroupID**

该属性只能用于Finding and Binding发起者的端点。它包含发起者将放置目标端点的group的标识符。如果它等于 0xFFFF，则将创建个体（rather than group）bindings。该属性值可以在编译时使用宏 BDB\_COMMISSIONING\_GROUP\_ID 进行初始化。

要使用此属性，需要在 u8bdbCommissioningMode 属性中启用Finding and Binding。

commissioning mode的Finding and Binding在 2.2.4 节中描述。


#### **u8bdbCommissioningMode**

该属性是一个 bitmap，用于指示端点上启用了哪些commissioning mode，其中每个位对应一种mode，并且当mode启用时对应位置为 ‘1’。这意味着如果需要的话，节点将能够实现该commissioning mode。该属性值可以在编译时使用宏 BDB\_COMMISSIONING\_MODE 来初始化。下表说明 bitmap 及用于置位的enumerations。

| Bit   | Commissioning Mode   | Enumeration                                   |
| :---: | :------------------- | :-------------------------------------------- |
| 0     | Touchlink            | BDB\_COMMISSIONING\_MODE\_TOUCHLINK           |
| 1     | Network Steering     | BDB\_COMMISSIONING\_MODE\_NWK\_STEERING       |
| 2     | Network Formation    | BDB\_COMMISSIONING\_MODE\_NWK\_FORMATION      |
| 3     | Finding and Binding  | BDB\_COMMISSIONING\_MODE\_FINDING\_N\_BINDING |
| 4-7   | 保留（设置为 ‘0’） | -                                             |

commissioning modes在 2.2 节中描述。

> Note：该属性用于所有节点类型。但是，为了启用commissioning mode，它必须在节点上是可用的，通过属性 u8bdbNodeCommissioningCapability 表明。启用的commissioning modes将成为节点commissioning capabilities的一个子集。


#### **ebdbCommissioningStatus**

该属性指示当前正在端点上进行的commissioning过程的状态。该属性取 BDB\_teCommissioningStatus 枚举中定义的值的其中一个（参见 2.8.2 节）。该属性用于所有节点类型。该属性值由ZBD实现内部更新，但可由应用程序读取。


#### **u64bdbJoiningNodeEui64**

该属性包含正在加入集中式安全网络的节点的64-bit IEEE/MAC地址，它仅用于Co-ordinator。该属性值由ZBD实现内部更新。


#### **au8bdbJoiningNodeNewTCLinkKey**

该属性包含一个新的链接密钥，用于当前正在加入网络但尚未被授予完整网络成员资格的节点。该属性值由ZBD实现内部更新（在加入节点及其父系节点上）。


#### **bbdbJoinUsesInstallCodeKey**

该属性指示在节点被允许加入网络之前，预配置链接密钥是否必须可用，这可能是预安装的链接密钥，也可能是派生自install code。值为TRUE表示需要链接密钥，FALSE表示不需要链接密钥。它仅用于 Co-ordinator/Trust Centre。该属性值可以在编译时使用宏 BDB\_JOIN\_USES\_INSTALL\_CODE\_KEY 进行初始化。默认情况下，该属性被设置为FALSE。该属性不被ZBD使用，如果其设置为TRUE，则应用程序有责任直接处理该功能并设置所需的密钥（参阅 u8bdbNodeJoinLinkKeyType）。


#### **u8bdbNodeCommissioningCapability**

该属性是指示节点的commissioning capabilities的 bitmap，其中每个位对应一种commissioning capabilities，并且如果capability存在则被置为 ‘1’。该属性用于所有节点类型。应用程序不能直接写入这些位，它们根据应用程序的makefile中定义的选项进行设置。下表详细介绍了 bitmap 和相关的makefile选项。

| Bit   | Capability           | Makefile 选项（如果定义了其中一个，则置为 ‘1’）                                                                                |
| :---: | :------------------- | :------------------------------------------------------------------------------------------------------------------------------- |
| 0     | Network Steering     | BDB\_SUPPORT\_NWK\_STEERING                                                                                                      |
| 1     | Network Formation    | BDB\_SUPPORT\_NWK\_FORMATION                                                                                                     |
| 2     | Finding and Binding  | BDB\_SUPPORT\_FIND\_AND\_BIND\_INITIATOR \| BDB\_SUPPORT\_FIND\_AND\_BIND\_TARGET                                                |
| 3     | Touchlink            | BDB\_SUPPORT\_TOUCHLINK\_INITIATOR\_END\_DEVICE \| BDB\_SUPPORT\_TOUCHLINK\_INITIATOR\_ROUTER \| BDB\_SUPPORT\_TOUCHLINK\_TARGET |
| 4-7   | 保留（设置为 ‘0’） | -                                                                                                                                |

以上commissioning modes在 2.2 节中描述。

> Note：为了使用其中一种可用的commissioning mode，必须通过属性 u8bdbCommissioningMode 来启用该mode。启用的commissioning modes将会成为节点commissioning capabilities的一个子集。


#### **bbdbNodeIsOnANetwork**

该属性指示本地节点当前是否网络的成员。值为TRUE表示它在网络中（但不一定绑定到任何远程节点），FALSE表示它不在网络中。该属性用于所有节点类型，但ZBD不维持它。应用程序负责维持属性值，并在重启后初始化属性（在调用任何其他ZBD函数之前）。


#### **u8bdbNodeJoinLinkKeyType**

该属性指示链接密钥的类型。当节点加入一个新的网络时，节点能够解密通过over-air接收到的已加密的网络密钥。该属性由Router和End Device节点使用。下表列出了属性值和对应的链接密钥类型，以及可用于定义链接密钥的宏。

| 值    | 链接密钥类型                                 | 链接密钥定义宏                                   |
| :---: | :------------------------------------------- | :----------------------------------------------- |
| 0x00  | Default Global Trust Centre Link Key         | DEFAULT\_GLOBAL\_TRUST\_CENTER\_LINK\_KEY        |
| 0x01  | Distributed Security Global Link Key         | DISTRIBUTED\_SECURITY\_GLOBAL\_LINK\_KEY         |
| 0x02  | Install Code Derived Pre-configured Link Key | INSTALL\_CODE\_DERIVED\_PRECONFIGURED\_LINK\_KEY |
| 0x03  | Touchlink Pre-configured Link Key            | TOUCHLINK\_PRECONFIGURED\_LINK\_KEY              |


#### **u32bdbPrimaryChannelSet**

该属性指定将在信道扫描中使用的2.4GHz无线信道的主要（第一选择）集合。该属性是一个 bitmap，其中每个位对应一个信道，如果该信道将被扫描则应该置为 ‘1’。位编号直接对应信道编号，即第11位对应2.4GHz的11信道，第26位对应26信道。该属性用于所有节点类型。该属性值可以在编译时使用宏 BDB\_PRIMARY\_CHANNEL\_SET 来初始化。


#### **u8bdbScanDuration**

该属性决定每个2.4GHz无线信道扫描操作的持续时间。实际的扫描时间计算如下：

$$aBaseSuperframeDuration  * (2^{bdbScanDuration} + 1)$$

其中 aBaseSuperframeDuration 在IEEE 802.15.4规范中定义。

该属性用于所有节点类型。该属性值取自ZPS配置编辑器中设置的扫描持续时间（Scan Duration Time）。


#### **u32bdbSecondaryChannelSet**

该属性指定将在信道扫描中使用的2.4GHz无线信道的次要（第二选择）集合。如果主要信道的扫描不成功，将使用此信道集。如果不需要扫描次要信道，则应该将该属性设置为零。该属性用于所有节点类型。该属性值可以在编译时使用宏 BDB\_SECONDARY\_CHANNEL\_SET 来初始化。


#### **u8bdbTCLinkKeyExchangeAttempts**

该属性指示当节点加入网络时请求（创建）新链接密钥的尝试次数。该属性用于Router和End Device节点。该属性值可以在编译时使用宏 BDB\_TC\_LINK\_KEY\_EXCHANGE\_ATTEMPTS 进行初始化。


#### **u8bdbTCLinkKeyExchangeAttemptsMax**

该属性指定当节点加入新的网络时，在密钥建立被放弃之前将进行的密钥建立尝试的最大次数。该属性用于Router和End Device节点。该属性值可以在编译时使用宏 BDB\_TC\_LINK\_KEY\_EXCHANGE\_ATTEMPTS\_MAX 进行初始化。


#### **u8bdbTCLinkKeyExchangeMethod**

该属性指定节点加入网络时用于获取新链接密钥的方法。下表列出了属性值及相应的方法。该属性用于Router和End Device节点。

| 值        | 密钥交换方法               |
| :-------: | :------------------------- |
| 0x00      | APS 请求密钥               |
| 0x01      | 基于证书的密钥交换（CBKE） |
| 0x02-0xFF | 保留                       |

该属性值可以在编译时使用宏 BDB\_TC\_LINK\_KEY\_EXCHANGE\_METHOD 来初始化。它应该被初始化为 0x00（APS 请求密钥）。


#### **u8bdbTrustCenterNodeJoinTimeout**

该属性指定Trust Centre在新加入节点的密钥建立失败时删除其Trust Centre生成链接密钥的超时时间（以秒为单位）。该属性仅用于 Co-ordinator/Trust Centre。该属性值可以在编译时使用宏 BDB\_TRUST\_CENTER\_NODE\_JOIN\_TIMEOUT 来初始化。


#### **bbdbTrustCenterRequireKeyExchange**

该属性指定Trust Centre是否要求加入节点将其初始链接密钥替换为由Trust Centre生成的新链接密钥。值为TRUE表示加入节点必须成功完成链接密钥交换过程，否则将从网络中移除该节点；值为FALSE表示加入节点将被允许保留在网络中，即使它没有完成链接密钥交换过程。该属性值可以在编译时使用宏 BDB\_TRUST\_CENTER\_REQUIRE\_KEYEXCHANGE 来初始化。它应该根据网络中实施的Trust Centre策略进行初始化，默认情况下，将其设置为FALSE以实现向后兼容。


#### **bTLStealNotAllowed**

这是NXP私有的标志，应用程序可以设置该标志以防止来自不同网络中的另一个节点的Touchlink commissioning指令 ‘stealing’ 本地节点。清除标志为允许节点be stolen，在这种情况下，它会离开当前网络并加入另一个网络或形成一个新的分布式网络，如Touchlink发起者所指示的那样。


#### **bLeaveRequested**

这是NXP私有的标志，应用程序应该只读而不写入。如果Touchlink commissioning操作导致ZBD发起网络离开，则该标志由Base Device设置。当生成 ZPS\_EVENT\_NWK\_LEAVE\_CONFIRM stack event 时，应用程序应该读取此标志，如果它为TRUE，则应用程序不应处理该event（因为ZBD将处理该event）。


### **2.5.2 Constants**

ZBD常量分为两类：
* 在所有节点上使用的，见 2.5.2.1 节
* 在支持Touchlink的节点上使用的，见 2.5.2.2 节


#### **2.5.2.1 General Constants**

下表列出了可以在所有节点上使用的ZBD常量，并且还展示了用于在 **bdb\_options.h** 文件中定义常量值的相应宏。

| 常量                            | 宏                                        |
| :------------------------------ | :---------------------------------------- |
| bdbcMaxSameNetworkRetryAttempts | BDBC\_MAX\_SAME\_NETWORK\_RETRY\_ATTEMPTS |
| bdbcMinCommissioningTime        | BDBC\_MIN\_COMMISSIONING\_TIME            |
| bdbcRecSameNetworkRetryAttempts | BDBC\_REC\_SAME\_NETWORK\_RETRY\_ATTEMPTS |
| bdbcTCLinkKeyExchangeTimeout    | BDBC\_TC\_LINK\_KEY\_EXCHANGE\_TIMEOUT    |


##### **bdbcMaxSameNetworkRetryAttempts**

该常量指定节点可以在同一网络上进行的加入或密钥交换尝试的最大次数。此常量的值使用宏 BDBC\_MAX\_SAME\_NETWORK\_RETRY\_ATTEMPTS 定义，并应设置为10（ZigBee BDB规范中的建议）。


##### **bdbcMinCommissioningTime**

该常量指定网络开放的最小时间间隔（以秒为单位），以允许新的节点加入或设备自我标识。此常量的值使用宏 BDBC\_MIN\_COMMISSIONING\_TIME 定义，并应设置为180（ZigBee BDB规范中的建议）。


##### **bdbcRecSameNetworkRetryAttempts**

该常量指定节点可以在同一网络上进行的加入或密钥交换尝试的建议次数。此常量的值使用宏 BDBC\_REC\_SAME\_NETWORK\_RETRY\_ATTEMPTS 定义，并应设置为3（ZigBee BDB规范中的建议）。


##### **bdbcTCLinkKeyExchangeTimeout**

该常量指定 APS 密钥请求发送到Trust Centre后加入节点将等待响应的最长时间（以秒为单位）。此常量的值使用宏 BDBC\_TC\_LINK\_KEY\_EXCHANGE\_TIMEOUT 定义，并应设置为5（ZigBee BDB规范中的建议）。


#### **2.5.2.2 Touchlink Constants**

下表列出了可用于支持Touchlink commissioning的节点上的ZBD常量，并且还展示了用于在 **bdb\_options.h** 文件中定义常量值的相应宏。

| 常量                          | 宏                                       |
| :---------------------------- | :--------------------------------------- |
| bdbcTLInterPANTransIdLifetime | BDBC\_TL\_INTERPAN\_TRANS\_ID\_LIFETIME  |
| bdbcTLMinStartupDelayTime     | BDBC\_TL\_MIN\_STARTUP\_DELAY\_TIME      |
| bdbcTLPrimaryChannelSet       | BDBC\_TL\_PRIMARY\_CHANNEL\_SET          |
| bdbcTLRxWindowDuration        | BDBC\_TL\_RX\_WINDOW\_DURATION           |
| bdbcTLScanTimeBaseDuration    | BDBC\_TL\_SCAN\_TIME\_BASE\_DURATION\_MS |
| bdbcTLSecondaryChannelSet     | BDBC\_TL\_SECONDARY\_CHANNEL\_SET        |


##### **bdbcTLInterPANTransIdLifetime**

此常量指定inter-PAN transaction ID保持有效的最长时间（以秒为单位）。此常量的值是使用宏 BDBC\_TL\_INTERPAN\_TRANS\_ID\_LIFETIME 定义的，应该设置为8（ZigBee BDB Specification中的建议）。


##### **bdbcTLMinStartupDelayTime**

此常量指定Touchlink发起者等待目标完成其网络启动过程的时间长度（以秒为单位）。此常量的值使用宏 BDBC\_TL\_MIN\_STARTUP\_DELAY\_TIME 定义，并应设置为2（ZigBee BDB Specification中的建议）。


##### **bdbcTLPrimaryChannelSet**

此常量指定将用于non-extended Touchlink扫描使用的主要（首选）2.4GHz无线信道集的 bitmap。这个常量的值是使用宏 BDBC\_TL\_PRIMARY\_CHANNEL\_SET 定义的，应该设置为 0x02108800，对应于通道11，15，20和25（ZigBee BDB Specification中的建议）。


##### **bdbcTLRxWindowDuration**

此常量指定在Touchlink commissioning期间节点的无线接收器保持启用状态的最大持续时间（以秒为单位），以便接收响应。此常量的值是使用宏 BDBC\_TL\_RX\_WINDOW\_DURATION 定义的，应该设置为5（ZigBee BDB Specification中的建议）。


##### **bdbcTLScanTimeBaseDuration**

该常量指定在Touchlink扫描操作期间发送一个扫描请求后节点的无线接收器将保持启用状态的基本持续时间（以毫秒为单位），以便接收响应。此常量的值是使用宏 BDBC\_TL\_SCAN\_TIME\_BASE\_DURATION\_MS 定义的，应该设置为250（ZigBee BDB Specification中的建议）。


##### **bdbcTLSecondaryChannelSet**

此常量指定将用于extended Touchlink扫描使用的次要（第二选择）2.4GHz无线信道集的 bitmap。它应该包含来自 bdbcTLPrimaryChannelSet 中指定的那些信道。此常量的值使用宏 BDBC\_TL\_SECONDARY\_CHANNEL\_SET 定义，并应设置为 0x07FFF800 异或 BDBC\_TL\_PRIMARY\_CHANNEL\_SET。


## **2.6 Functions**

本节详细介绍为ZBD提供的C函数。
* BDB_vInit
* BDB_vSetKeys
* BDB_vStart
* BDB_eNfStartNwkFormation
* BDB_eNsStartNwkSteering
* BDB_eFbTriggerAsInitiator
* BDB_vFbExitAsInitiator
* BDB_eFbTriggerAsTarget
* BDB_vFbExitAsTarget
* BDB_bIsBaseIdle
* BDB_u8OutOfBandCommissionStartDevice
* BDB_vOutOfBandCommissionGetData
* BDB_eOutOfBandCommissionGetDataEncrypted
* BDB_bOutOfBandCommissionGetKey

> Note 1：应用程序必须提供一个用户定义的回调函数 APP_vBdbCallback() 以处理 ZBD events。该函数原型在第 2.9 节中给出。

> Note 2：ZBD提供回调函数 BDB_vZclEventHandler()，它在Finding and Binding过程期间处理某些 ZCL events，如第 2.2.4 节所述。


### **BDB_vInit**

```c
/*
 * Function: BDB_vInit
 * Description: 该函数初始化 ZBD，并且必须是您的代码中调用的第一个 ZBD 函数
 *              在调用 ZPS_eAplAfInit() 初始化 ZigBee PRO stack 后必须调用该函数
 *              在调用该函数之前，ZBD 属性 bbdbNodeIsOnANetwork 必须从持久存储器中恢复(if relevant)
 * Parameter: psInitArgs -> Handle of the ZigBee Base Device event queue
 * Return: None
 */
void BDB_vInit(BDB_tsInitArgs *psInitArgs);
```

> Note：在调用此函数之前，应用程序必须使用ZigBee PRO Stack库中的函数 ZTIMER_eInit() 来初始化所需的ZigBee软件计时器。这样做时，它必须添加一些定时器供ZBD内部使用，其中这个数字由宏 BDB_ZTIMER_STORAGE 定义。

该函数执行的初始化包括以下内容：
* 将ZBD属性设置为默认值，除非应用程序在文件 **bdb_options.h** 中定义了其他值
* 注册传递到此函数的ZBD消息队列 - ZBD使用此消息队列捕获 stack events
* 调用 BDB_vSetKeys() 以根据其节点类型设置初始预配置安全密钥（在文件 **bdb_link_keys.c** 中定义）：
    * For a Co-ordinator, the Default Global Trust Centre Link Key is set
    * For a Router or End Device, both the Default Global Trust Centre Link Key and Distributed Security Global Link Key are set
* 打开ZBD内部使用的定时器

更多关于安全密钥的信息，请参阅 2.3 节。


### **BDB_vSetKeys**

```c
/*
 * Function: BDB_vSetKeys
 * Description: 该函数将本地节点上适当的预配置链接密钥加载到内存中，用于节点的初始安全状态
 *              该函数由 BDB_vInit() 自动调用
 *              但是在复位后，可能需要显式调用该函数才能恢复链接密钥，从而移除内存中的密钥
 * Parameter: None
 * Return: None
 */
void BDB_vSetKeys(void);
```

加载的链接密钥类型取决于节点类型，如下所示：
* On a Co-ordinator, the Default Global Trust Centre Link Key is loaded for participation in a centralised security network
* On a Router or End Device, both of the following keys are loaded:
    * Default Global Trust Centre Link Key for participation in a centralised security network
    * Distributed Security Global Link Key for participation in a distributed security network

预配置链接密钥在文件 **bdb_link_keys.c** 中定义。网络安全在 2.3 节中描述。


### **BDB_vStart**

```c
/*
 * Function: BDB_vStart
 * Description: 该函数启动 ZBD
 *              必须在 BDB_vInit() 之后和应用程序进入主循环（e.g. APP_vMainLoop()）之前调用
 * Parameter: None
 * Return: None
 */
void BDB_vStart(void);
```

根据节点类型以及其之前是否为网络成员，该函数可能会执行操作，也可能不执行操作。无论哪种情况，该函数都会最终使用合适的event调用回调函数 APP_vBdbCallback()。

* If the node was not in a network:
    * 对于支持Touchlink commissioning的Router节点，该函数从为Touchlink定义的主要信道集中选择节点的无线信道
    * 对于其他Router，Co-ordinator和End Device节点，不执行任何操作，应用程序随后需要将节点加入到网络

在上述情况下，该函数将生成 BDB_EVENT_INIT_SUCCESS event。

* If the node was in a network:
    * 对于Co-ordinator和Router节点，不执行任何操作，并且生成 BDB_EVENT_INIT_SUCCESS event
    * 对于End Device节点，将执行一系列重新加入尝试，如果尝试成功，则生成 BDB_EVENT_REJOIN_SUCCESS event；如果失败，则生成 BDB_EVENT_REJOIN_FAILURE event。除非启用了unsecured joins，其将尝试通过Network Steering进行加入。

上述行为的更多详情在 2.1 节中描述。


### **BDB_eNfStartNwkFormation**

```c
/*
 * Function: BDB_eNfStartNwkFormation
 * Description: 该函数启动 Network Formation 过程（如果需要）
 *              该函数必须在 BDB_vStart() 之后调用
 *              如果节点需要，则必须通过属性 u8bdbCommissioningMode 启用 Network Formation
 * Parameter: None
 * Return: BDB_E_SUCCESS                 -> Network Formation 已成功启动
 *         BDB_E_ERROR_INVALID_PARAMETER -> End Device has attempted Network Formation
 *         BDB_E_ERROR_NODE_IS_ON_A_NWK  -> 节点已在网络中
 */
BDB_teStatus BDB_eNfStartNwkFormation(void);
```

该函数仅可被Co-ordinator或Router调用：
* If called on a Co-ordinator, a centralised security network will be formed
* If called on a Router, a distributed security network will be formed

上述网络类型在 2.3 节中描述。

一旦Network Formation开始，该函数将返回并且Network Formation过程的最终结果将由一个 asynchronous event 指示 - 以下之一：
* BDB_EVENT_NWK_FORMATION_SUCCESS if a centralised or distributed network has been successfully formed
* BDB_EVENT_NWK_FORMATION_FAILURE if a network has not been successfully formed

Network Formation在 2.2.3 节中详细描述。


### **BDB_eNsStartNwkSteering**

```c
/*
 * Function: BDB_eNsStartNwkSteering
 * Description: 该函数启动 Network Steering 过程（如果需要）
 *              该函数必须在 BDB_vStart() 之后调用
 *              如果节点需要，则必须通过属性 u8bdbCommissioningMode 启用 Network Steering
 * Parameter: None
 * Return: BDB_E_SUCCESS                           -> Network Steering 已成功启动
 *         BDB_E_ERROR_IMPROPER_COMMISSIONING_MODE -> Network Steering 没启用
 *         BDB_E_ERROR_COMMISSIONING_IN_PROGRESS   -> 节点已处于 commissioning mode
 *         BDB_E_ERROR_INVALID_DEVICE              -> 加入节点是一个 Co-ordinator
 */
BDB_teStatus BDB_eNsStartNwkSteering(void);
```

该函数执行的操作取决于本地节点是否已经是网络的成员：
* 当节点已经在网络中并且是Co-ordinator或Router，它将开放网络以供其他节点加入。默认时间为180秒，可以使用文件 **bdb_options.h** 中的宏 BDBC_MIN_COMMISSIONING_TIME 配置此时间。
* 当节点不在网络中时，它将搜索合适的网络并且尝试加入。节点加入网络后，该节点将进行身份验证并从父系节点中接收网络密钥。如果网络具有Trust Centre，则节点可以将其预配置链接密钥替换为由Trust Centre生成并提供的链接密钥。

一旦Network Steering开始后，该函数将返回并且Network Steering过程的最终结果将由一个 asynchronous event 指示 - 以下之一：
* BDB_EVENT_NWK_STEERING_SUCCESS if Network Steering has been completed successfully
* BDB_EVENT_NO_NETWORK if no open network was discovered for joining
* BDB_EVENT_NWK_JOIN_FAILURE if the node attempted to join a network but failed

Network Steering在 2.2.2 节中详细描述。


### **BDB_eFbTriggerAsInitiator**

```c
/*
 * Function: BDB_eFbTriggerAsInitiator
 * Description: 该函数启动发起者端点上的 Finding and Binding 过程
 *              该函数可能作为用户操作的结果被调用，例如按下按钮
 *              发起者将在一个固定的时间内保持 Finding and Binding mode
 *              该固定时间使用文件 bdb_options.h 中的宏 BDBC_MIN_COMMISSIONING_TIME 定义（以秒为单位）
 * Parameter: u8SourceEndPointId -> Number of initiator endpoint
 * Return: BDB_E_SUCCESS                         -> Finding and Binding 已成功启动
 *         BDB_E_FAILURE                         -> 无效的端点号或无法广播 Identify Query 指令
 *         BDB_E_ERROR_COMMISSIONING_IN_PROGRESS -> Finding and Binding 已正在进行
 */
BDB_teStatus BDB_eFbTriggerAsInitiator(uint8 u8SourceEndPointId);
```

发起者节点首先通过广播 Identify Query 指令来搜索目标端点。如果发起者从远程端点接收到响应，则它会向此端点发送 Simple Descriptor 请求。如果成功接收所请求的 Simple Descriptor，则发起者将检查此descriptor以查找远程端点上的簇是否与其自身的簇匹配。如果至少有一个匹配的簇，发起者执行以下操作之一：
* 如果需要binding（通过属性 u16bdbCommissioningGroupID 等于 0xFFFF 来指示），则发起者将远程端点添加到本地的Binding table上
* 如果需要grouping（通过属性 u16bdbCommissioningGroupID 等于一个16-bit group地址来指示），则发起者将请求目标端点将group地址添加到其Group Address table上

Finding and Binding mode在 2.2.4 节中描述。

> Note：在该函数的调用期间产生的events在 2.2.4.1 节中详细描述。


### **BDB_vFbExitAsInitiator**

```c
/*
 * Function: BDB_vFbExitAsInitiator
 * Description: 该函数会停止发起者端点上正在进行的 Finding and Binding 过程
 *              该函数可能作为用户操作的结果被调用，例如按下按钮/释放按钮
 * Parameter: None
 * Return: None
 */
void BDB_vFbExitAsInitiator(void);
```

Finding and Binding mode在 2.2.4 节中描述。


### **BDB_eFbTriggerAsTarget**

```c
/*
 * Function: BDB_eFbTriggerAsTarget
 * Description: 该函数启动目标端点上的 Finding and Binding 过程
 *              该函数必须由目标端点上的应用程序在本地调用
 *              该函数可能作为用户操作的结果被调用，例如按下按钮
 *              设备将在一定时间内置于 identification mode 中
 *              该时间至少等于 bdb_options.h 中的宏 BDBC_MIN_COMMISSIONING_TIME 定义的值（以秒为单位）
 *              在此期间，目标设备将生成对 Identify Query 指令以及其他 Finding and Binding 指令的响应
 * Parameter: u8EndPoint -> Number of target endpoint
 * Return: BDB_E_SUCCESS -> Finding and Binding 已成功启动
 *         BDB_E_FAILURE -> 无效的端点号或 Identify cluster 无法访问
 */
BDB_teStatus BDB_eFbTriggerAsTarget(uint8 u8EndPoint);
```

随后可以在本地使用函数 BDB_vFbExitAsTarget() 将端点带离Find and Binding mode，或者使用 Identify cluster 函数 eCLD_IdentifyCommandIdentifyRequestSend() 以离开（通过发起者）。

Finding and Binding mode在 2.2.4 节中描述。


### **BDB_vFbExitAsTarget**

```c
/*
 * Function: BDB_vFbExitAsTarget
 * Description: 该函数会停止目标端点上正在进行的 Finding and Binding 过程
 *              该函数必须由目标端点上的应用程序在本地调用
 *              该函数可能作为用户操作的结果被调用，例如按下按钮/释放按钮
 * Parameter: u8SourceEndpoint -> Number of target endpoint
 * Return: None
 */
void BDB_vFbExitAsTarget(uint8 u8SourceEndpoint);
```

Finding and Binding mode在 2.2.4 节中描述。


### **BDB_bIsBaseIdle**

```c
/*
 * Function: BDB_bIsBaseIdle
 * Description: 该函数用以指示 ZBD 的忙或空闲状态，以判断节点是否可以进入睡眠模式
 *              如果 ZBD 处于空闲状态，则应用程序需要负责将 KW41Z 置于睡眠模式
 * Parameter: None
 * Return: TRUE  -> 指示 ZBD 处于空闲状态，节点可以睡眠
 *         FALSE -> 指示 ZBD 处于忙状态
 */
bool_t BDB_bIsBaseIdle(void);
```


### **BDB_u8OutOfBandCommissionStartDevice**

```c
/*
 * Function: BDB_u8OutOfBandCommissionStartDevice
 * Description: 该函数用于启动 out-of-band commissioning
 *              以允许本地设备作为 Co-ordinator 形成一个网络，或作为 Router/End Device 加入现有网络
 *              该函数应该在 ZPS_eAplAfInit() 之后调用
 *              当通过 out-of-band 的方式从其他设备接收到 commissioning 数据时，该函数会被调用
 * Parameter: psStartupData -> 指向包含 commissioning 数据的 structure 的指针（见 2.7.5 节）
 * Return: BDB_E_SUCCESS                 -> 设备成功形成或加入网络
 *         BDB_E_FAILURE                 -> 形成或加入网络的请求没有被接受
 *         ZPS_NWK_ENUM_INVALID_REQUEST  -> 请求包含无效数据
 *         ZPS_APL_APS_E_ILLEGAL_REQUEST -> stack 未处于接受请求的正确状态
 */
uint8 BDB_u8OutOfBandCommissionStartDevice(BDB_tsOobWriteDataToCommission *psStartupData);
```

必须将此commissioning数据在structure BDB_tsOobWriteDataToCommission（第 2.7.5 节中所述）中提供给函数（并非所有数据值都是强制性的）。

out-of-band commissioning接口对数据值作出合理的假定，并且不允许commissioning数据将节点中已有的某些值覆盖。例如：
* 其不允许设置Co-ordinator的网络地址为非零值（Co-ordinator的网络地址必须为零）
* 其不允许设置Co-ordinator的重新加入标志（因为Co-ordinator不能离开并重新加入网络）
* 在集中式网络中，其不允许设置Trust Centre的IEEE/MAC地址为非Co-ordinator的IEEE/MAC地址（因为Co-ordinator始终为Trust Centre）

有关out-of-band commissioning的概述，请参阅 2.2.5 节。


### **BDB_vOutOfBandCommissionGetData**

```c
/*
 * Function: BDB_vOutOfBandCommissionGetData
 * Description: 该函数用以获取本地存储的 commissioning 数据
 * Parameter: psReturnedCommissioningData -> 指向获取接收到的 commissioning 数据的 structure 的指针
 *                                                                               （见 2.7.6 节）
 * Return: None
 */
void BDB_vOutOfBandCommissionGetData(BDB_tsOobReadDataToAuthenticate *psReturnedCommissioningData);
```

获得的数据以第 2.7.6 节中描述的structure接收，并包含网络密钥。然后数据可以传递到更高层，在将其通过out-of-band的方式发送到其他参与commissioning的设备之前，可以将数据加密。

使用函数 BDB_eOutOfBandCommissionGetDataEncrypted() 可以获得类似的数据集，但网络密钥已被加密。

有关out-of-band commissioning的概述，请参阅 2.2.5 节。


### **BDB_eOutOfBandCommissionGetDataEncrypted**

```c
/*
 * Function: BDB_eOutOfBandCommissionGetDataEncrypted
 * Description: 该函数用以获取本地存储的 commissioning 数据，包含将被加密返回的网络密钥
 *              必须提供将用于加密网络密钥的认证数据（包括 install code）
 * Parameter: psSrcCredentials  -> 指向包含用于加密网络密钥的认证数据的 structure 的指针（见 2.7.7 节）
 *            pu8ReturnAuthData -> 指向包含获取数据的返回字节流的开始位置
 *            puSize            -> 指向接收获得的字节流大小的位置的指针
 * Return: None
 */
BDB_teStatus BDB_eOutOfBandCommissionGetDataEncrypted(
                                            BDB_tsOobWriteDataToAuthenticate *psSrcCredentials,
                                            uint8 *pu8ReturnAuthData,
                                            uint16 *puSize);
```

获得的数据以字节流形式接收 - 字节流的大小也会返回。 字节流包含以下数据：
* IEEE/MAC address of the local node as u64address (8 bytes)
* Network key encrypted with the data passed via psSrcCredentials (16 bytes)
* MIC value generated to validate encryption (4 bytes)
* Key sequence number of active network key (1 byte)
* Active channel number (1 byte)
* PAN ID (2 bytes)
* Extended PAN ID (8 bytes)

然后可以将已加密的网络密钥和其他获得的数据通过out-of-band的方式发送到其他参与commissioning的设备。接收设备可以使用函数 BDB_bOutOfBandCommissionGetKey() 来解密密钥。

使用函数 BDB_u8OutOfBandCommissionStartDevice() 可以获得没有已加密网络密钥的类似数据集。

有关out-of-band commissioning的概述，请参阅 2.2.5 节。


### **BDB_bOutOfBandCommissionGetKey**

```c
/*
 * Function: BDB_bOutOfBandCommissionGetKey
 * Description: 该函数用以解密已加密的安全密钥（在 out-of-band commissioning 期间接收到的网络密钥）
 *              该函数需要 install code，以生成加密密钥的预配置链接密钥
 * Parameter: pu8InstallCode -> 指向 install code 的指针
 *            pu8EncKey      -> 指向原设备的 IEEE/MAC 地址的指针
 *            pu8DecKey      -> 指向接收已解密密钥的位置的指针
 *            pu8Mic         -> 指向 MIC 值的指针，用于验证解密
 * Return: TRUE  -> 解密成功
 *         FALSE -> 解密失败
 */
bool_t BDB_bOutOfBandCommissionGetKey(uint8* pu8InstallCode,
                                      uint8* pu8EncKey,
                                      uint64 u64ExtAddress,
                                      uint8* pu8DecKey,
                                      uint8* pu8Mic);
```

有关out-of-band commissioning的概述，请参阅 2.2.5 节。


## **2.7 Structures**

### **2.7.1 BDB_tsBdbEvent**

以下structure包含传递给回调函数 APP_vBdbCallback() 的 ZBD event 信息（参见第2.9节）。
```c
typedef struct
{
    BDB_teBdbEventType eEventType;
    BDB_tuBdbEventData uEventData;
} BDB_tsBdbEvent;
```

where:
* eEventType 是指示event类型的enumeration - 对于可能的enumerations，请参见第 2.9 节
* uEventData 是包含event信息（if any）的union structure - 有关此structure的描述，请参阅第 2.7.2 节


### **2.7.2 BDB_tuBdbEventData**

以下structure是包含 ZBD event 数据的union。
```c
typedef union
{
    BDB_tsZpsAfEvent sZpsAfEvent;
    BDB_tsFindAndBindEvent *psFindAndBindEvent;
} BDB_tuBdbEventData;
```

where:
* sZpsAfEvent 是一个包含 stack event 数据的structure，由event类型 BDB_EVENT_ZPSAF 指示 - 有关此structure的说明，请参见第 2.7.3 节
* psFindAndBindEvent 是指向包含 “Finding and Binding” event 数据的structure的指针（参见第 2.9 节） - 有关此structure的描述，请参见第 2.7.4 节


### **2.7.3 BDB_tsZpsAfEvent**

以下structure包含 ZigBee stack event 的数据（请参见部分）。
```c
typedef struct
{
    uint8 u8EndPoint;
    ZPS_tsAfEvent sStackEvent;
} BDB_tsZpsAfEvent;
```

where:
* u8EndPoint 是发生event的端点号
* sStackEvent 是一个包含 stack event 类型和数据的 ZPS structure - 该structure在 ZigBee 3.0 Stack User Guide (ZB3STAUG) 中有详细介绍


### **2.7.4 BDB_tsFindAndBindEvent**

以下structure包含 “Finding and Binding” event（参见第 2.9 节）的数据，该数据在发起者的Finding and Binding过程中传递给应用程序。
```c
typedef struct{
    uint8 u8InitiatorEp;
    uint8 u8TargetEp;
    uint16 u16TargetAddress;
    uint16 u16ProfileId;
    uint16 u16DeviceId;
    uint8 u8DeviceVersion;
    union {
        uint16 u16ClusterId;
        uint16 u16GroupId;
    } uEvent;
    ZPS_tsAfZdpEvent *psAfZdpEvent;
    bool bAllowBindOrGroup;
    bool bGroupCast;
} BDB_tsFindAndBindEvent;
```

where:
* u8InitiatorEp 是发起者节点上 binding/grouping 所涉及的端点号
* u8TargetEp 是目标节点上 binding/grouping 所涉及的端点号
* u16TargetAddress 是目标节点的16-bit网络地址
* u16ProfileId 是两个节点支持的ZigBee application profile的标识符(for Lighting & Occupancy devices, this is 0x0104)
* u16DeviceId 是目标端点支持的ZigBee设备类型的16-bit标识符。这必须是由ZigBee联盟发布的设备类型标识符
* u8DeviceVersion 包含4位（bits 0-3），表示目标节点上支持的设备描述的版本（默认值为0000，除非根据所用的application profile设置为其他值）
* uEvent 是以下两个字段的union：
    * u16ClusterId 是binding中涉及的簇的标识符
    * u16GroupId 是目标端点将分配到的group的地址
* psAfZdpEvent 是指向包含生成的 Finding and Binding event 的 structure ZPS_tsAfZdpEvent 的指针 - 该 ZPS structure 在 ZigBee 3.0 Stack User Guide (ZB3STAUG) 中详细说明。该event可以是以下任何一种（详见 2.9 节）：
    * BDB_EVENT_FB_HANDLE_SIMPLE_DESC_RESP_OF_TARGET
    * BDB_EVENT_FB_CHECK_BEFORE_BINDING_CLUSTER_FOR_TARGET
    * BDB_EVENT_FB_CLUSTER_BIND_CREATED_FOR_TARGET
    * BDB_EVENT_FB_BIND_CREATED_FOR_TARGET
    * BDB_EVENT_FB_GROUP_ADDED_TO_TARGET
    * BDB_EVENT_FB_ERR_BINDING_FAILED
    * BDB_EVENT_FB_ERR_BINDING_TABLE_FULL
    * BDB_EVENT_FB_ERR_GROUPING_FAILED
    * BDB_EVENT_FB_NO_QUERY_RESPONSE
    * BDB_EVENT_FB_TIMEOUT
* bAllowBindOrGroup 是一个布尔类型标志，用于指示相关簇是否被允许参与 binding/grouping。默认值为TRUE（允许），但如果应用程序需要排除簇（并阻止 binding/grouping），则应将此字段设置为FALSE
* bGroupCast 是一个布尔类型标志，用于指示是否应向所有标识目标广播 “Add Group If Identifying” 指令（TRUE），还是应将 “Add Group” 请求单独发送给所有标识目标。默认值是TRUE


### **2.7.5 BDB_tsOobWriteDataToCommission**

以下structure包含用于在节点的out-of-band commissioning开始时初始化节点的数据值。
```c
typedef struct {
    uint64 u64PanId;
    uint64 u64TrustCenterAddress;
    uint8* pu8NwkKey;
    uint8* pu8InstallCode;
    uint16 u16PanId;
    uint16 u16ShortAddress;
    bool_t bRejoin;
    uint8 u8ActiveKeySqNum;
    uint8 u8DeviceType;
    uint8 u8RxOnWhenIdle;
    uint8 u8Channel;
    uint8 u8NwkUpdateId;
} BDB_tsOobWriteDataToCommission;
```

where:
* u64PanId 是要加入的网络的Extended PAN ID
* u64TrustCenterAddress 是要加入的集中式网络中Trust Center的IEEE/MAC地址
* pu8NwkKey 是一个指向网络密钥的指针
* pu8InstallCode 是一个指向从install code派生的初始链接密钥的指针（见 2.3.1 节）
* u16PanId 是要加入的网络的PAN ID
* u16ShortAddress 是分配给该节点的网络地址
* bRejoin 是 “rejoin flag”，它表示节点是否应该尝试重新加入网络（TRUE: rejoin, FALSE: do not rejoin）
* u8ActiveKeySqNum 是与活动网络密钥关联的密钥序列号
* u8DeviceType 是指示ZigBee节点类型的值（其他值保留）:
    * 0: Co-ordinator
    * 1: Router
    * 2: End Device
* u8RxOnWhenIdle 是指示节点的接收器在空闲期间是否启用的值（其他值保留）:
    * 0: Receiver off when idle (sleeping device)
    * 1: Receiver on when idle (non-sleeping device)
* u8Channel 是网络运行所在的无线信道号
* u8NwkUpdateId 是一个唯一的字节值，它在网络参数更新时递增（因此用于确定接收节点是否错过了更新）


### **2.7.6 BDB_tsOobReadDataToAuthenticate**

以下structure包含在节点的out-of-band commissioning期间从本地节点读取的数据值。
```c
typedef struct {
    uint8 au8Key[16]__attribute__((aligned (16)));
    uint64 u64TcAddress;
    uint64 u64PanId;
    uint16 u16ShortPanId;
    uint8 u8ActiveKeySeq;
    uint8 u8Channel;
} BDB_tsOobReadDataToAuthenticate;
```

where:
* au8Key[16]\_\_attribute\_\_((aligned (16))) 是一个包含当前网络密钥的数组，每个数组元素一个字节
* u64TcAddress 是节点正在commissioned的网络的Trust Centre的IEEE/MAC地址
* u64PanId 是该节点正在commissioned的网络的Extended PAN ID
* u16ShortPanId 是该节点正在commissioned的网络的PAN ID
* u8ActiveKeySeq 是当前活动网络密钥的密钥序列号
* u8Channel 是网络运行所在的无线信道号


### **2.7.7 BDB_tsOobWriteDataToAuthenticate**

以下structure包含用于在节点的out-of-band commissioning期间加密安全密钥的认证数据。
```c
typedef struct {
    uint64 u64ExtAddr;
    uint8* pu8InstallCode;
} BDB_tsOobWriteDataToAuthenticate;
```

where:
* u64ExtAddr 是节点的IEEE/MAC地址
* pu8InstallCode 是一个指向要在密钥加密中使用的16-byte install code的指针


## **2.8 Enumerations**

本节列出并描述了ZBD上使用的enumerations。However，第 2.9 节详细介绍了 ZBD event enumerations。


### **2.8.1 BDB_teStatus**

以下enumerations用于指示某些函数调用的状态。
```c
typedef enum
{
    BDB_E_SUCCESS,
    BDB_E_FAILURE,
    BDB_E_ERROR_INVALID_PARAMETER,
    BDB_E_ERROR_INVALID_DEVICE,
    BDB_E_ERROR_NODE_IS_ON_A_NWK,
    BDB_E_ERROR_IMPROPER_COMMISSIONING_MODE,
    BDB_E_ERROR_COMMISSIONING_IN_PROGRESS,
} BDB_teStatus;
```

enumerations在下表中列出并描述。

| Enumeration                             | Description                                |
| :-------------------------------------- | :----------------------------------------- |
| BDB_E_SUCCESS                           | 函数调用的目的是成功的                     |
| BDB_E_FAILURE                           | 函数调用失败，并且没有其他错误代码是合适的 |
| BDB_E_ERROR_INVALID_PARAMETER           | 指定的参数值无效                           |
| BDB_E_ERROR_INVALID_DEVICE              | 设备类型对操作无效                         |
| BDB_E_ERROR_NODE_IS_ON_A_NWK            | 节点已经在网络中                           |
| BDB_E_ERROR_IMPROPER_COMMISSIONING_MODE | commissioning mode 不合适                  |
| BDB_E_ERROR_COMMISSIONING_IN_PROGRESS   | commissioning 过程正在进行中               |


### **2.8.2 BDB_teCommissioningStatus**

以下enumerations用于指示节点的commissioning过程的状态。
```c
typedef enum
{
    E_BDB_COMMISSIONING_STATUS_SUCCESS,
    E_BDB_COMMISSIONING_STATUS_IN_PROGRESS,
    E_BDB_COMMISSIONING_STATUS_NOT_AA_CAPABLE,
    E_BDB_COMMISSIONING_STATUS_NO_NETWORK,
    E_BDB_COMMISSIONING_STATUS_FORMATION_FAILURE,
    E_BDB_COMMISSIONING_STATUS_NO_IDENTIFY_QUERY_RESPONSE,
    E_BDB_COMMISSIONING_STATUS_BINDING_TABLE_FULL,
    E_BDB_COMMISSIONING_STATUS_NO_SCAN_RESPONSE,
    E_BDB_COMMISSIONING_STATUS_NOT_PERMITTED,
    E_BDB_COMMISSIONING_STATUS_TCLK_EX_FAILURE
} BDB_teCommissioningStatus;
```

enumerations在下表中列出并描述。

| Enumeration                                           | Description                        |
| :---------------------------------------------------- | :--------------------------------- |
| E_BDB_COMMISSIONING_STATUS_SUCCESS                    | Commissioning 已成功完成           |
| E_BDB_COMMISSIONING_STATUS_IN_PROGRESS                | Commissioning 正在进行中           |
| E_BDB_COMMISSIONING_STATUS_NOT_AA_CAPABLE             | 父系无法将地址分配给加入节点       |
| E_BDB_COMMISSIONING_STATUS_NO_NETWORK                 | 没有发现可以加入的网络             |
| E_BDB_COMMISSIONING_STATUS_FORMATION_FAILURE          | Network formation 失败             |
| E_BDB_COMMISSIONING_STATUS_NO_IDENTIFY_QUERY_RESPONSE | 没有收到 Identify Query 指令的响应 |  |
| E_BDB_COMMISSIONING_STATUS_BINDING_TABLE_FULL         | 本地 Binding table 已满            |
| E_BDB_COMMISSIONING_STATUS_NO_SCAN_RESPONSE           | 在信道扫描期间未收到任何响应       |
| E_BDB_COMMISSIONING_STATUS_NOT_PERMITTED              | 不允许请求 commissioning           |
| E_BDB_COMMISSIONING_STATUS_TCLK_EX_FAILURE            | Trust Centre 链接密钥交换失败      |


## **2.9 Events**

ZBD有许多关联的events。 一些API函数（在第 2.6 节中描述）会立即返回，并且其调用过程的结果稍后会生成一个 asynchronous event 来指示。必须在应用程序中定义用户定义的回调函数以处理这些events。这个回调函数的原型如下：
```c
void APP_vBdbCallback(BDB_tsBdbEvent *psBdbEvent)
```

其中 psBdbEvent 是指向包含要传递给函数的event信息的 BDB_tsBdbEvent event structure 的指针（对于此structure，请参见第 2.7.1 节）。

下面列出了 ZBD events 的enumerations。
```c
typedef enum {
    BDB_EVENT_NONE,
    BDB_EVENT_ZPSAF,
    BDB_EVENT_INIT_SUCCESS,
    BDB_EVENT_REJOIN_SUCCESS,
    BDB_EVENT_REJOIN_FAILURE,
    BDB_EVENT_NWK_STEERING_SUCCESS,
    BDB_EVENT_NO_NETWORK,
    BDB_EVENT_NWK_JOIN_SUCCESS,
    BDB_EVENT_NWK_JOIN_FAILURE,
    BDB_EVENT_APP_START_POLLING,
    BDB_EVENT_NWK_FORMATION_SUCCESS,
    BDB_EVENT_NWK_FORMATION_FAILURE,
    BDB_EVENT_FB_HANDLE_SIMPLE_DESC_RESP_OF_TARGET,
    BDB_EVENT_FB_CHECK_BEFORE_BINDING_CLUSTER_FOR_TARGET,
    BDB_EVENT_FB_CLUSTER_BIND_CREATED_FOR_TARGET,
    BDB_EVENT_FB_BIND_CREATED_FOR_TARGET,
    BDB_EVENT_FB_GROUP_ADDED_TO_TARGET,
    BDB_EVENT_FB_ERR_BINDING_FAILED,
    BDB_EVENT_FB_ERR_BINDING_TABLE_FULL,
    BDB_EVENT_FB_ERR_GROUPING_FAILED,
    BDB_EVENT_FB_NO_QUERY_RESPONSE,
    BDB_EVENT_FB_TIMEOUT,
    BDB_EVENT_FB_OVER_AT_TARGET,
    BDB_EVENT_LEAVE_WITHOUT_REJOIN,
} BDB_teBdbEventType;
```

这些events如下所述。

在 “Finding and Binding” 过程中使用名称中带有 “FB” 的events，event数据包含在 BDB_tsFindAndBindEvent structure中（参见第 2.7.4 节）。

> Note：此外，某些 ZCL events 在Finding and Binding过程中生成，并传递给随 ZBD 提供的回调函数BDB_vZclEventHandler()。有关这些events，请参阅第 2.2.4 节。


### **BDB_EVENT_ZPSAF**

此event指示发生了 ZigBee stack event。在这种情况下，uEventData 字段（of the BDB_tsBdbEvent structure）包含 BDB_tsZpsAfEvent structure，该structure本身包含 ZPS_tsAfEvent stack event structure。


### **BDB_EVENT_INIT_SUCCESS**

该event在ZBD成功初始化时生成。


### **BDB_EVENT_REJOIN_SUCCESS**

该event在节点成功重新加入其先前的网络时生成。


### **BDB_EVENT_REJOIN_FAILURE**

当节点尝试重新加入其先前的网络失败时，会生成此event。


### **BDB_EVENT_NWK_STEERING_SUCCESS**

当Network Steering过程成功完成并且本地节点已广播以下任一消息时，会生成此event：
* Management Permit Joining 消息以请求开放网络让其他设备加入（当本地节点在Network Steering之前已在网络中时广播此消息）
* Device Announce 消息以宣布本地节点刚加入网络（当本地节点在Network Steering之前不在网络中时广播此消息）


### **BDB_EVENT_NO_NETWORK**

当尝试加入网络的设备执行信道扫描后未发现开放的网络时，会生成此event。


### **BDB_EVENT_NWK_JOIN_SUCCESS**

该event在节点成功加入网络时生成。


### **BDB_EVENT_NWK_JOIN_FAILURE**

当节点尝试加入网络但失败时生成此事件。


### **BDB_EVENT_APP_START_POLLING**

该event在Trust Centre链接密钥交换过程期间生成（在End Device上），指示应用程序启动对其父系的快速轮询，以检索接收到的数据包（作为交换过程一部分）。


### **BDB_EVENT_NWK_FORMATION_SUCCESS**

当本地节点已成功形成集中式或分布式网络时，此event在Network Formation过程结束时生成。


### **BDB_EVENT_NWK_FORMATION_FAILURE**

如果本地节点无法形成网络，则会在Network Formation过程结束时生成此event。


### **BDB_EVENT_FB_HANDLE_SIMPLE_DESC_RESP_OF_TARGET**

此event指示发起者已收到来自目标的 Simple Descriptor 响应。应用程序可以使用此event来确定发起者binding到哪种类型的设备（例如，Dimmable Light，On / Off Light）。提供给应用程序的信息是：
* u8InitiatorEp
* u8TargetEp
* u16TargetAddress
* u16ProfileId
* u16DeviceId
* u8DeviceVersion
* psAfZdpEvent (points to received Simple Descriptor)


### **BDB_EVENT_FB_CHECK_BEFORE_BINDING_CLUSTER_FOR_TARGET**

此event仅在为簇创建Binding table条目之前生成。它通过将 bAllowBindOrGroup 标志设置为FALSE（默认为TRUE），以为应用程序提供将簇从binding中排除的机会。当应用程序需要通过将属性 u16bdbCommissioningGroupID 设置为 0xFFFF 以外的值来执行group binding时，也可以使用此event。此外，此event还允许应用程序决定是否通过将 bGroupCast 设置为TRUE（默认情况下假定为FALSE）来向所有标识目标广播 “Add Group If Identifying” 指令或单独向所有标识目标单播一个 “Add Group” 请求。提供给应用程序的信息是：
* u8InitiatorEp
* u8TargetEp
* u16TargetAddress
* u16ClusterId
* bAllowBindOrGroup
* bGroupCast
* psAfZdpEvent (points to received Simple Descriptor)


### **BDB_EVENT_FB_CLUSTER_BIND_CREATED_FOR_TARGET**

每次创建 binding/grouping 都会生成此event。对于同一个目标设备，该event可能会多次生成。例如，将一个 Color Dimmer Switch binding 到 Dimmable Light 时，该event将生成两次：一次用于 On/Off Cluster，一次用于 Level Control Cluster。提供给应用程序的信息是：
* u8InitiatorEp
* u8TargetEp
* u16TargetAddress
* u16ClusterId


### **BDB_EVENT_FB_BIND_CREATED_FOR_TARGET**

一旦所有地址bindings完成，就会生成此event。然后，应用程序可以向bound目标发送 “Stop Identifying” 指令。提供给应用程序的信息是：
* u8InitiatorEp
* u8TargetEp
* u16TargetAddress


### **BDB_EVENT_FB_GROUP_ADDED_TO_TARGET**

一旦发送了 “Add Group” 或 “Add Group If Identifying”，就会生成此event，以通知应用程序已从发起者的角度完成grouping。然后，应用程序可以将 “Stop Identifying” 指令组播到grouped目标。提供给应用程序的信息是：
* u8InitiatorEp
* u8TargetEp
* u16GroupId
* u16TargetAddress
* psAfZdpEvent


### **BDB_EVENT_FB_ERR_BINDING_FAILED**

生成此event以指示创建Binding table条目时发生意外错误。


### **BDB_EVENT_FB_ERR_BINDING_TABLE_FULL**

生成此event以通知应用程序Binding table已满，因此Finding and Binding过程失败。结果，ZBD将退出Finding and Binding过程。


### **BDB_EVENT_FB_ERR_GROUPING_FAILED**

生成此event以指示grouping失败，因为发起者无法发送 “Add Group” 或 “Add Group If Identifying” 请求。


### **BDB_EVENT_FB_NO_QUERY_RESPONSE**

此event指示发起者在 BDB_FB_RESEND_IDENTIFY_QUERY_TIME（默认值为10）秒内未收到 Identify Query 响应。提供给应用程序的信息是：
* u8InitiatorEp


### **BDB_EVENT_FB_TIMEOUT**

此event指示commissioning计时器在通过常量 BDBC_MIN_COMMISSIONING_TIME（默认为180秒）定义的一段时间后过期。提供给应用程序的信息是：
* u8InitiatorEp


### **BDB_EVENT_FB_OVER_AT_TARGET**

此event指示Finding and Binding过程已在目标节点上结束，因为标识时间已达到零或远程节点已将其强制为零。


### **BDB_EVENT_LEAVE_WITHOUT_REJOIN**

当节点已被指示离开网络而未尝试重新加入网络时，会生成此event。


## **2.10 Compile-time Options**

编译时选项可以通过文件 **bdb_options.h** 中的定义进行配置。这允许为ZBD属性和常量定义自定义值。如果在此文件中未定义属性或常量的值，则将使用该属性或常量的默认值。


### **Attributes**

以下宏可用于预配置ZBD属性的值（在第 2.5.1 节中列出和描述）：
* BDB_COMMISSIONING_GROUP_ID
* BDB_COMMISSIONING_MODE
* BDB_COMMISSIONING_STATUS
* BDB_JOINING_NODE_EUI64
* BDB_JOIN_USES_INSTALL_CODE_KEY
* BDB_NODE_JOIN_LINK_KEY_TYPE
* BDB_PRIMARY_CHANNEL_SET
* BDB_SCAN_DURATION
* BDB_SECONDARY_CHANNEL_SET
* BDB_TC_LINK_KEY_EXCHANGE_ATTEMPTS
* BDB_TC_LINK_KEY_EXCHANGE_ATTEMPTS_MAX
* BDB_TC_LINK_KEY_EXCHANGE_METHOD
* BDB_TRUST_CENTER_NODE_JOIN_TIMEOUT
* BDB_TRUST_CENTER_REQUIRE_KEYEXCHANGE

例如，要将密钥建立尝试的最大次数设置为5，请包括以下行：
```c
#define BDB_TC_LINK_KEY_EXCHANGE_ATTEMPTS_MAX  5
```


### **Constants**

以下宏可用于设置ZBD常量的值（在第 2.5.2 节中列出和描述）：
* BDBC_MAX_SAME_NETWORK_RETRY_ATTEMPTS
* BDBC_MIN_COMMISSIONING_TIME
* BDBC_REC_SAME_NETWORK_RETRY_ATTEMPTS
* BDBC_TC_LINK_KEY_EXCHANGE_TIMEOUT
* BDBC_TL_INTERPAN_TRANS_ID_LIFETIME
* BDBC_TL_MIN_STARTUP_DELAY_TIME
* BDBC_TL_PRIMARY_CHANNEL_SET
* BDBC_TL_RX_WINDOW_DURATION
* BDBC_TL_SCAN_TIME_BASE_DURATION_MS
* BDBC_TL_SECONDARY_CHANNEL_SET

例如，要将网络打开加入的最短commissioning时间设置为240秒，请包括以下行：
```c
#define BDBC_MIN_COMMISSIONING_TIME  240
```

（这个最小commissioning时间应该设置为低于255秒的值）

---
