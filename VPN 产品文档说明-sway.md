## 结构##
- **app**

- **依赖库**
  - 核心： `libVPN`
  - 统计

    - `libUmeng`：友盟统计 sdk

    - `libStat`：二次封装
  - 广告
    - `libDuQuickCharge`：百度原生广告+充电锁屏广告 sdk
    - `libMobVista`：MobVista 广告 sdk
  - 支付： `libPayment` 

![依赖关系](依赖关系.png)



## 流程##

-    **核心**：

     - **`VpnAgent.java`**

       ```       
       // Initialize
       void init(this);
       void prepare();

       // Status
       boolean isPrepared();
       boolean isConnected();

       // Listener
       void addVpnListener(vpnListener);
       void removeVpnListener(vpnListener);

       // Action
       void connect();
       void disconnect();
       ```

     - **`VpnListener.java`**

       ```
       
       void onPrepared();

       void onAuth(Intent intent);

       void onConnecting(VpnNode currentNode);
       void onConnected(VpnNode currentNode);
       void onDisconnected(VpnNode currentNode);

       boolean tryNextPort(int nextPort, String proto);
       boolean tryNextNode(VpnNode nextNode);
       ```

     - **`VpnNode.java`**
-    **激活**： `ActivateUserThread.java`

-    **获取服务器**： `CheckServerThread.java`   
     - 优先级
        - `API` —> `FireBase` —> `Cache` — > `Apk`
     - 质量
        - `sdk 逻辑`: 按 load 值，每个国家选 3 台作为本次服务器列表
        - `app`: ping 服务器，根据算法得出 score，score 越高越好（本次 ping ）


## 配置
- AndroidManifest.xml  
-
` <meta-data
      android:name="vpn_sdk_api"
      android:value="http://pro.allconnected.in/abc"/>
  <meta-data
      android:name="vpn_sdk_method"
      android:value="/query/servers_list_pro/"/>
  <meta-data
      android:name="vpn_opt_method"
      android:value="/query/servers_list_pro_opt/"/>
<service  
android:name="co.allconnected.lib.openvpn.OpenVpnService"  
android:exported="false"  
android:permission="android.permission.BIND_VPN_SERVICE">  
<intent-filter>  
<action android:name="android.net.VpnService"/>
</intent-filter>
</service>`

## 数据##

- 核心指标  

  - **激活成功率**

    - 分母：独立用户数

    > Snap 重构后版本约 80%

  - **拨号成功率**

    > Snap 重构后版本约 75%

  - **留存率：**

    > Snap Android 约 40%

  - **错误率：**

    > 0.5% 以下

