---
title: WMS - 仓库管理系统
tags: [TODO]
date: 2019-11-27 14:24:25
---

# 一、公共

## 1.1 接口
### 1.1.1 查询仓库列表
/inventory/inventoryWH/queryAllWarehourseSimpleList

### 1.1.2 插入处理日志

/wms/common/insertProcessLog



## 1.2 方法

### 1.2.1 查询商品图片列表 - 仓库图片展示

com.yamibuy.wms.cloud.service.ImageService.queryItemImgList(String)

### 1.2.2 根据库位编码查询库位信息

com.yamibuy.wms.cloud.dao.wms.LocationDao.queryLocationByLocationNo(String, String)

### 1.2.3 库位状态标记

com.yamibuy.wms.cloud.service.LocationService.markLocation(MarkLocationRequest)

库位状态：（参见LocationFlagEnum）

| 库位状态 | 库位状态值 | 描述     |
| -------- | ---------- | -------- |
| NORMAL   | 0          | 正常     |
| OOS      | 1          | 缺货     |
| QUESTION | 2          | 问题库位 |
| OCCUPIED | 3          | 已占用   |

### 1.2.4 邮件发送

com.yamibuy.central.core.util.SendMailUtil.send(Sendmail sendMail)

传入模板名称及参数，方法调用hub服务发送邮件

参考：通用邮件模板开发 -  https://yamibuy.atlassian.net/wiki/spaces/Dev/pages/272367639

相关数据表：yamibuy_central.template

## 1.3 数据库表

### 1.3.1 yamibuy_wh

- wh_config
  - 仓库配置
- wh_inventory_transaction
- wh_storage_location
  - TODO 库位状态枚举 flag
- wh_location_type
- wh_order_pool
  - TODO 状态枚举 status
- wh_order_info
  - TODO 订单状态枚举 order_status
  - TODO 配送状态枚举 shipping_status
  - TODO 支付状态枚举 pay_status
- wh_order_goods

### 1.3.2 yamibuy_inventory

- yamibuy_inventory.inventory_transaction
- yamibuy_inventory.inventory_transaction_ex

### 1.3.3 yamibuy_po

- yamibuy_po.po_purchase_order
- yamibuy_po.po_purchase_order_transaction

### 1.3.4 yamibuy_im

- yamibuy_im.im_item
- yamibuy_im.im_item_short_description
- yamibuy_im.im_item_extend
- yamibuy_im.im_pm
- yamibuy_im.im_category
- yamibuy_im.im_item_upc
- yamibuy_im.im_item_warehouse_image

### 1.3.5 yamibuy_central

- yamibuy_central.config
- yamibuy_central.holiday




# 二、Central

## 2.1 可用库位

### 2.1.1 功能
1. 查看可用Stock
2. 查看可用Bin

### 2.1.2 路由
viewAvaliableSB

### 2.1.3 接口
- /wms/service/queryAvaiBinList
- /wms/service/queryAvaiStockList

### 2.1.4 数据表
- wh_storage_location


## 2.2 损益管理
库位库存管理
库位状态：正常; OOS; 有问题; 已占用

### 2.2.1 功能
1. 查看仓库商品库位库存
2. 调整仓库商品库位库存
3. 仓库商品新增库位
4. 查看商品破损图片
5. 仓库商品库位锁定

### 2.2.2 路由
invList
invDetail

### 2.2.3 接口
- /wms/adjust/queryLocationInvList
- /wms/adjust/confirmLocationInv
- /wms/location/markLocation

### 2.2.4 数据表
- wh_inventory_transaction


## 2.3 补货任务

### 2.3.1 功能
1. 查看补货任务
2. Excel导入补货任务
3. Excel导入调整补货任务优先级
4. 新增补货任务
5. 修改补货任务信息
6. 删除补货任务

### 2.3.2 路由
restockTaskList

### 2.3.3 接口
/wms/restock/queryRestockTaskList

### 2.3.4 数据表
- wh_restock_pool


## 2.4 商品配置
商品配置包括：过期容忍值，Bin上限，Stock上限

### 2.4.1 功能
1. 查看商品配置
2. 添加商品配置
3. 修改商品配置

### 2.4.2 路由
itemSetting

### 2.4.3 接口
/wms/itemsettings/queryItemSetingList

### 2.4.4 数据表
- wh_item_setting


## 2.5 补单管理

### 2.5.1 功能
1. 查看补单列表
2. 查看补单历史
3. 查看补单商品详细信息

### 2.5.2 路由
replenishManagement

### 2.5.3 接口
/wms/replenish/queryReplenishList

### 2.5.4 数据表
- wh_replenish_order
- wh_replenish_order_item
- wh_replenish_picking_tote


## 2.6 数据监控

### 2.6.1 功能
1. 查看影响订单的补货任务
2. 补货任务优先级调整
3. 查看异常补货中转区
4. 查看MoveIn中转区商品列表
5. 查看PST商品列表
6. 查看待转PST商品列表

### 2.6.2 路由
dashBoard

### 2.6.3 接口
- /wms/dashBoard/queryReplenishTaskList
- /wms/dashBoard/queryNeededClearList
- /wms/dashBoard/queryMoveInList
- /wms/dashBoard/queryPstList
- /wms/dashBoard/queryNoOrderToteList

### 2.6.4 数据表
- wh_restock_pool
- wh_order_info
- wh_order_pool
- wh_picking_order


## 2.7 Bin释放
如果Bin库位为当前商品最后一个Bin，则保留，不允许释放
TODO 应与损益管理合并，库位管理 - 包括库位损益和释放与商品解绑功能

### 2.7.1 功能
1. 查看仓库商品无库存Bin库位
2. 释放无库存Bin库位
3. 批量释放无库存Bin库位

### 2.7.2 路由
releaseBin

### 2.7.3 接口
- /wms/location/queryOccurpiedBinList
- /wms/location/releaseBin

### 2.7.4 数据表
- TODO 迁移 SP：up_release_bin_v3
- wh_storage_location_item


## 2.8 错收冲减
业务划分：PO采购错收; FBY收货错收
商品划分：好货冲减；坏货冲减

### 2.8.1 功能
1. 查看错收冲减任务列表
2. 新建错收冲减任务
3. 错收冲减

### 2.8.2 路由
wrongReceiveManagement
wrongReceiveTask

### 2.8.3 接口
- /wms/receiveReturn/batchList
- /wms/receiveReturn/itemInfo
- /wms/receiveReturn/inventoryList
- TODO 接口token问题 /wms/receiveReturn/opt
    - 调用PO接口，调整PO收货数量
    - 调用Inventory库存接口，调整商品收货数量

### 2.8.4 数据表
- wh_inbound
- wh_inbound_batch
- 
- yamibuy_po.po_vendor
- yamibuy_po.po_purchase_order
- yamibuy_po.po_purchase_order_transaction
- 
- yamibuy_master.xysc_vendor_info
- wh_seller_shipment
- wh_seller_shipment_transaction
- 
- TODO 规格问题 yamibuy_master.xysc_goods - pieces_per_pack
- TODO PO已上架商品数量问题 wh_inventory_transaction_log


## 2.9 盘点列表
盘点类型：Adhoc; IRDR; W2W
盘点轮次：一盘; 二盘; 三盘

### 2.9.1 功能
1. 查看盘点任务列表
2. Excel导入盘点任务
3. 添加IRDR盘点任务
4. 修改盘点任务优先级
5. 处理盘点任务
6. 批量处理盘点任务
7. 调整盘点任务数量
8. TODO 删除盘点任务

### 2.9.2 路由
checkStockMan

### 2.9.3 接口
/wms/check/queryCountInfoList

### 2.9.4 数据表
- wh_count_info
- wh_storage_location_item
- wh_count_type


## 2.10 设备版本管理
WMS APP 各版本使用的设备
TODO WMS PICKING APP 未接入
TODO 控制菜单权限

### 2.10.1 功能
1. 查看仓库设备版本
2. TODO 修改设备版本 可移除功能
3. 查看仓库APP历史版本
    TODO 弹窗打开速度慢
4. 添加仓库APP版本
    TODO 指定版本是否强制升级
5. TODO 按当前版本筛选
6. TODO 版本设为最新不区分仓库

### 2.10.2 路由
deviceInfoManagement

### 2.10.3 接口
- /wms/deviceInfo/queryList
- /wms/deviceInfo/historyList

### 2.10.4 数据表
- wh_device_info
- wh_config
- wh_app_versions
- 
- yamibuy_inventory.inventory_warehouse


## 2.11 仓库订单统计

### 2.11.1 功能
1. 仓库订单汇总统计
    当天已发货订单
    未发货普通订单; 未发货预售订单
    订单流程统计：未支付; 等待拣货; 拣货中; 等待打包; 打包中; 补单
    Batch统计：类型分为S L XL PW
2. 查看仓库拣货单列表
3. 查看拣货单详情
4. 查看Batch列表
5. Batch中移除订单
6. 置顶Batch优先拣货, 调整Batch优先级
7. 查看等待Batch订单列表
8. 查看仓库拣货配置，分普通拣货和Put Wall拣货
9. 修改仓库拣货配置

### 2.11.2 路由
orderRecord

### 2.11.3 接口
- /wms/orderCount/orderCard/001
- /wms/batch/queryPickingOrderList
- /wms/batch/queryBatchesByCondition
- /wms/batch/queryPoolOrderInfos
- /wms/common/queryConfigs/000
- /wms/batch/order?queryNO=104874200&warehouse_number=001

### 2.11.4 数据表
- wh_order_process_record
- wh_order_pool
- wh_order_info
- wh_order_goods
- wh_batch
- wh_batch_order
- wh_picking_task
- wh_picking_order
- wh_picking_order_item
- wh_replenish_order
- wh_shipping_method
- wh_shipping_carrier
- wh_config
- 
- ymb_wh.wall_slots
- 
- yamibuy_master.xysc_admin_user
- yamibuy_master.xysc_wearhouse_goods


## 2.12 打印收货收据
TODO 仓库新增功能 - 收货管理，包括收货信息展示，错收冲减和收货收据打印

### 2.12.1 功能
1. 打印PO收货收据

### 2.12.2 路由
receipt

### 2.12.3 接口
/po/purchaseOrder/queryPoDetailByNumber

### 2.12.4 数据表
- yamibuy_po.po_purchase_order
- yamibuy_po.po_purchase_order_transaction


## 2.13 配送管理

### 2.13.1 功能
1. 查看配送方式
2. 锁定配送方式
3. 修改配送方式配置
4. 删除配送方式 
5. 查看运输公司
   1. UPS
   2. USPS
   3. Purolator
   4. OnTrac
   5. LaserShip
   6. Gesoo
   7. 7 Hours
6. 修改运输公司配置
7. 添加运输公司
8. 查看配送关系 - 前后台配送关系管理
9. 设置后台默认配送方式
10. 修改配送关系
11. 删除配送关系
12. 添加配送关系
13. TODO 添加配送方式

### 2.13.2 路由
shippingManage

### 2.13.3 接口
- /wms/shippingMethod/list
- /wms/shippingMethod/carrier/list
- /wms/shippingMethod/mapping/list


### 2.13.4 数据表
- wh_shipping_method
- wh_shipping_carrier
- wh_shipping_mapping


## 2.14 邮编管理
TODO 移入配送管理，删除此菜单

### 2.14.1 功能
1. 查看配送方式zipcode信息
2. 修改配送方式zipcode信息
3. 添加配送方式覆盖zipcode

### 2.14.2 路由
zipCodeManage

### 2.14.3 接口
/wms/shippingMethod/zipInfo/list

### 2.14.4 数据表
- wh_shipping_zipinfo
- wh_shipping_method
- wh_shipping_carrier


## 2.15 Cost管理
TODO 移入配送管理，删除此菜单

### 2.15.1 功能
1. 查看配送方式cost信息 - 运费成本
2. 修改配送方式cost信息
3. 添加配送方式cost
    TODO 添加页面zone排序

### 2.15.2 路由
costManage

### 2.15.3 接口
/wms/shippingMethod/cost/list

### 2.15.4 数据表
- wh_shipping_cost
- wh_shipping_method


## 2.16 保质期管理

### 2.16.1 功能
1. 查看仓库商品保质期
2. Excel批量导入保质期检查任务
3. 批量添加保质期检查任务

### 2.16.2 路由
expManage

### 2.16.3 接口
/wms/expireDtm/queryList

### 2.16.4 数据表
- wh_shelf_task_pool
- 
- yamibuy_im.im_category_shelf_life
- yamibuy_im.im_pm_assignment

## 2.17 商品信息更新

### 2.17.1 功能

1. 仓库收货商品拍照流程功能
2. Check In
3. Check Out

相关库位：

| 库位名称        | 库位类型 | 库位描述                                                     |
| --------------- | -------- | ------------------------------------------------------------ |
| PIC tote        | 40       | Items have image or/and description issues, which are not taking photos or editing description |
| PIC In Transfer | 41       | Items have image or/and description issues, which are taking photos or editing description |

相关功能：

WMS APP - PIC Return

### 2.17.2 路由

itemInfoUpdate

### 2.17.3 接口

- /wms/pic/queryWaitCheckInItems
- /wms/pic/queryWaitCheckOutItems

### 2.17.4 数据表

- wh_item_pic_status
- wh_pic_tote_status

  


# 三、WMS APP

## 3.1 首页

### 3.1.1 功能
1. 登录
2. 修改服务连接地址
3. 注销
    - 取消已领取的补货任务
    - 取消已领取的盘点任务
    - 取消已领取的调拨拣货任务
    - 取消已领取的保质期检查任务
    - TODO 优化为单个注销接口

### 3.1.2 路由
login

### 3.1.3 接口
- /wms/common/login
    - 调用 hub服务登录接口 - /hub/admin/login
- /wms/common/queryLoginOperation?username=xxx
- /wms/print/queryMAC
- /wms/restockPick/cancelRestockTask
- /wms/check/releaseCountTask
- /wms/tsPicking/cancelTasks
- /wms/expireDtm/unBindTask


### 3.1.4 数据表
- wh_print_set
- yamibuy_central.role
- yamibuy_central.operation
- yamibuy_central.role_opera_mapping
- yamibuy_central.group
- yamibuy_central.group_role_mapping
- yamibuy_master.xysc_admin_user
- yamibuy_central.user_group_mapping


## 3.2 Settings 设置

### 3.2.1 功能
1. 修改密码
2. 绑定解绑蓝牙
3. 清除缓存

### 3.2.2 路由
settings
    - changePwd
        - bluetooth

### 3.2.3 接口
- /wms/warehouseUser/changePassword
    - 调用hub接口1 - /hub/users/warehouse
    - 调用hub接口2 - /hub/users/warehouseReset
- /wms/print/bindMAC


### 3.2.4 数据表
- wh_print_set

## 3.3 Pre-Receive 预收货

### 3.3.1 功能
1. PO预收货
2. SP（Seller Shippment）预收货
3. 生成蓝牙可识别的打印码
4. 查看收货历史
    - 长按可重复打印

### 3.3.2 路由
pre-receive

### 3.3.3 接口
- /wms/preReceive/checkPo
- /wms/preReceive/queryPreReceiveDetail
- /wms/preReceive/savePreReceiveAndImages

### 3.3.4 数据表
- wh_pre_receive
- wh_pre_receive_item
- wh_image
- 
- yamibuy_po.po_purchase_order
- yamibuy_po.po_purchase_order_transaction
- yamibuy_po.po_vendor
- yamibuy_po.po_vendor_shipping
- 
- wh_seller_shipment
- wh_seller_shipment_transaction
- 
- wh_print_set


## 3.4 Receive 收货

### 3.4.1 功能
1. PO收货
    - TODO 超过收货数量时无提示直接跳回输入UPC界面
2. SP收货
3. 按UPC收货
    - 完成预收货的PO可以按UPS进行收货
4. 查看收货日历
5. 收货时如果描述不符、图片不符，会触发拍照流程，并且加入收货挂起Pending列表
6. 坏货和过期商品会要求拍照
7. 采购系统变化：PO状态修改，商品收货数量改变，并重新计算商品平均成本
8. 库存系统变化：采购库存 ==> 收货库存 （坏货不增加收货库存）

### 3.4.2 路由
receive

### 3.4.3 接口
- /wms/receive/receiveSchedule
- /wms/receive/queryReceiveList
- /wms/receive/receiveDetail
- /wms/receive/receiveConfirm
    - 调用PO，收货挂起：/po/purchaseManage/insertPending
    - 

### 3.4.4 数据表
- wh_inbound
- wh_inbound_transaction
- wh_inbound_batch
- wh_image
- wh_item_setting - 保质期容忍天数 difference_day
- wh_pre_receive_item
- wh_transship_truck_po - 调拨
- 
- yamibuy_im.im_pm
- yamibuy_im.im_pm_assignment
- 
- yamibuy_master.xysc_wearhouse_goods - 校验商品长宽高
- yamibuy_master.xysc_goods 规格 - changwei 
- yamibuy_master.xysc_vendor_info - seller名称
- 
- yamibuy_po.po_purchase_order
- yamibuy_po.po_purchase_order_transaction
- yamibuy_po.po_vendor
- yamibuy_po.po_vendor_shipping
- yamibuy_po.po_upc_item_mapping
- yamibuy_po.po_vendor_product_wh
- 
- wh_seller_shipment
- wh_seller_shipment_transaction
- yamibuy_inventory.inventory_warehouse
- 
- wh_print_set


## 3.5 Put Away 上架

### 3.5.1 功能
1. 收货区商品上架
    - TODO 当前为按照PO和Seller Shipment进行收货
    - 调用PO接口，/po/purchaseManage/putaway - 修改PO收货数量
2. 商品Assign Stock
3. 商品Assign Bin
4. 上架优先顺序
    - 有stock时优先将商品上架到stock上
5. 检查库位过期日期， 更新库位过期日期
6. 检查库位容量
7. 获取商品规格
    - 上架规格取收货批次中的规格（收货时用户输入）

8. 上架后发送到货通知
   - 区分是否PM管控商品
9. 上架后更新商品网站展示缓存
   - TODO 判断是否需要修改


### 3.5.2 路由
putAway

### 3.5.3 接口
- /wms/putaway/queryPutawayList
- /wms/putaway/queryPOList
- /wms/putaway/queryLocationDetail
- /wms/service/queryAvaiLocationListNoCount
- /wms/putaway/assignDetail
- /wms/putaway/checkLocation
- /wms/putaway/assignConfirm
- /wms/putaway/queryConfirmDetail
- /wms/putaway/confirmLocation


### 3.5.4 数据表

- wh_inbound

- wh_inbound_transaction

- wh_process_log

- wh_inventory_transaction_log

- wh_storage_location_item

- wh_storage_location_item_log

- 

- wh_seller_shipment

- wh_seller_shipment_transaction

- yamibuy_po.po_purchase_order

- yamibuy_po.po_purchase_order_transaction

- yamibuy_po.po_vendor

  

## 3.6 Restock 补货

相关功能：

- Central - 仓库管理 - 补货任务；
- JOB - 补货任务生成

| 库位名称       | 库位类型 | 库位描述                                                     |
| -------------- | -------- | ------------------------------------------------------------ |
| Stock          | 3        | None pickable area, where GOOD inventory stores              |
| Bin            | 4        | Pickable area, where GOOD inventory stores                   |
| Restock Pallet | 38       | Restock pick pallet,Restock bin load pallet                  |
| Restock Slot   | 39       | Between restock pick and restock bin load processes, where items are |

### 3.6.1 功能
1. 补货拣货

   Stock库位过期日期信息会存入Redis中, Binload时从Redis中取出并存至Bin库位上

2. 将Stock库位上的商品移动到Pallet上，Pallet置于SLOT区域，Binload时Pallet与SLOT解绑后，从Pallet上将商品移入Bin库位

3. 任务完成或库位标记时可能会触发盘点任务生成

4. 相关功能：
   - Central - 仓库管理 - 补货任务；
   - JOB - 补货任务生成

### 3.6.2 路由

- restockPickGetList
- restockBinLoadList


### 3.6.3 接口
- /wms/restockPick/pickList
- /wms/restockPick/checkLocation
- /wms/restockPick/pickDetail
- /wms/restockPick/pickConfirm
- /wms/common/insertProcessLog
- /wms/restockPick/canReleasePallet
- /wms/restockPick/confirmChange
- 
- /wms/restockBinLoad/binLoadList
- /wms/restockBinLoad/checkSlot
- /wms/restockBinLoad/checkLeftUpc
- /wms/restockBinLoad/binLoadDetail
- /wms/restockBinLoad/binLoadConfirm
- /wms/restockBinLoad/binLoadComplete


### 3.6.4 数据表

- wh_restock_pool
- wh_process_log
- wh_storage_pallet
- wh_pallet_item
- wh_inbound
- wh_inbound_batch
- wh_replenish_order
- wh_replenish_order_item

- yamibuy_master.xysc_goods - 规格：changwei as pieces_per_pack



## 3.7 Replenish 补单

| 库位名称       | 库位类型 | 库位描述                                                     |
| -------------- | -------- | ------------------------------------------------------------ |
| Bin            | 4        | Pickable area, where GOOD inventory stores                   |
| Replenish Tote | 34       | After put wall sorting process, where incomplete order are   |
| Replenish Slot | 35       | Where replenish tote put to                                  |
| Hot-Pick Tote  | 26       | Items picked from sellable bin that are for incompleted orders |



### 3.7.1 功能

1. 补单拣货 - 订单进入仓库拣货流程，拣货人员未能从库位上拣到订单要求数量的商品，则订单进入补单流程；

   补单流程将会尝试从商品其他库位上进行拣货，以满足订单要求。

### 3.7.2 路由

- replenish
- orderCheckIn
- replenishPick
- itemCheckIn


### 3.7.3 接口

- /wms/replenish/checkTote
- /wms/replenish/orderDetail
- /wms/replenish/checkSlot
- /wms/replenish/bindSlot
- /wms/replenish/pickList
- /wms/replenish/checkHotTote
- /wms/replenish/pickDetail
- /wms/replenish/pickConfirm
- /wms/replenish/pickCompleted
- /wms/replenish/itemCheckInToteList
- /wms/replenish/itemCheckInList
- /wms/replenish/itemCheckInDetail
- /wms/replenish/itemCheckInConfirm
- /wms/replenish/unbindSlot


### 3.7.4 数据表

- wh_replenish_order
- wh_replenish_order_item
- wh_replenish_picking_tote
- wh_picking_order
- wh_picking_order_item



## 3.8 Adjust Tool 通用工具

### 3.8.1 功能

通用工具用于在库位间灵活调转商品。

可保留商品保质期信息。

可售库位 ==> 不可售库位，Inventory可用库存减少

不可售库位 ==> 可售库位，Inventory可用库存增加

### 3.8.2 路由

- tool
- moveOut
- moveIn
- tsMoveOut


### 3.8.3 接口

- /wms/general/checkMoveRules
- /wms/general/checkItemUPC
- /wms/general/isNeedAddReason
- /wms/general/confirmMove
- /wms/general/checkLocationIn
- /wms/general/selectTote
- TODO TS Pallet


### 3.8.4 数据表

- wh_movetool_rules
- yamibuy_master.xysc_goods - 规格 changwei as pieces_per_pack



## 3.9 EXP Check 保质期检查

### 3.9.1 功能

检查仓库库位商品保质期。

库位OOS时会添加Adhoc类型盘点任务。

相关功能：

- Central - 仓库管理 - 保质期管理

### 3.9.2 路由

- expCheckLocType


### 3.9.3 接口

- /wms/expireDtm/getTaskList
- /wms/expireDtm/queryLocationInfo
- /wms/expireDtm/queryItemInfoByUpc/{item_number}
- /wms/expireDtm/finishTask


### 3.9.4 数据表

- wh_shelf_task_pool
- yamibuy_po.po_vendor_product - PM ID - pm_user_id

- yamibuy_im.im_category

- yamibuy_im.im_category_shelf_life



## 3.10 Search 搜索

### 3.10.1 功能

支持按照商品名称，UPC，仓库商品库位查询商品信息。

### 3.10.2 路由

- search


### 3.10.3 接口

- /wms/search
- /wms/search/detail

### 3.10.4 数据表

- wh_storage_location_item

- yamibuy_im.im_item_warehouse_image
- yamibuy_im.im_item_upc



## 3.11 Gesoo

### 3.11.1 功能

Gesoo订单出库记录。

### 3.11.2 路由

- gesoo
- gesooOutbound
- gesooInbound

- gesooSearch


### 3.11.3 接口

- /wms/gesoo/outBound
- /wms/gesoo/search
- /wms/gesoo/inBound

### 3.11.4 数据表

- wh_gesoo
- wh_gesoo_log
- yamibuy_master.xysc_admin_user



## 3.12 PST Return

### 3.12.1 功能

出库前取消的订单商品在打包台从拣货Tote中转移到Problem Solve Tote，使用PST Return功能将PST Tote中的商品归还上架到Bin库位上；

如果商品没有Bin库位，可选择Assgn新Bin；

归还后，前台可售库存增加，Problem Solve Tote状态由占用变为正常

相关库位：

| 库位名称           | 库位类型 | 库位描述                 |
| ------------------ | -------- | ------------------------ |
| Bin                | 4        |                          |
| Problem Solve Tote | 36       | Where canceled order are |

相关库存字段：

wait_return_qty

### 3.12.2 路由

- pstReturn


### 3.12.3 接口

- /wms/pstReturn/pstList
- /wms/pstReturn/checkPSTote
- /wms/pstReturn/psToteInfo
- queryLocationList
- queryItemByLocation
- assignBinLocation
- confirmReturn
- returnComplete
  - 完成时，如Tote中仍有商品，直接损益处理

### 3.12.4 数据表

- wh_picking_order
- wh_storage_location_item



### 3.13 PIC Return 拍照框归还

### 3.13.1 功能

拍照完成且可售卖的商品使用此功能归还到Bin库位上

归还后，前台可售库存增加，PIC Tote状态由占用变为正常

相关库位：

| 库位名称 | 库位类型 | 库位描述                                                     |
| -------- | -------- | ------------------------------------------------------------ |
| Bin      | 4        |                                                              |
| PIC tote | 40       | Items have image or/and description issues, which are not taking photos or editing description |

相关库存字段：

pic_tote_qty
PIC Tote状态枚举：

| 状态值 | 状态描述 |
| -------- | -------- |
| 0     | 可以从RECG区放入或开始从transfer区放入 |
| 1 | check_in锁定 |
| 2 | 已经check_out未return bin |
| 3 | check_out因return bin锁定 |

### 3.13.2 路由

- picReturn


### 3.13.3 接口

- /wms/pic/queryAvalibaleTote4ReturnBin
- /wms/pic/queryItemInfoInPicTote
- com.yamibuy.wms.cloud.api.PicRest

### 3.13.4 数据表

- wh_pic_tote_status
- wh_item_pic_status



### 3.14 Empty Location 可用库位

### 3.14.1 功能

查看当前可用Stock / Bin库位。

功能同Central - 可用库位。

### 3.14.2 路由

- searchLocation


### 3.14.3 接口

- /wms/service/queryAvaiLocationList
- com.yamibuy.wms.cloud.api.AvailableLocationRest

### 3.14.4 数据表

- wh_location_size
- wh_item_fill_rate
- wh_count_info
- yamibuy_master.xysc_wearhouse_goods
- yamibuy_master.xysc_goods
- 

### 3.15 Cycle Count 盘点

TODO

### 3.16 Cycle Add 盘点任务创建

TODO

### 3.17 Problem Inventory 问题处理

TODO

### 3.18 TS Pick 调拨拣货

### 3.19 TS QC 调拨QC

### 3.20 Truck manage 调拨车管理

### 3.21 TS Receive 调拨收货

### 3.22 TS put away 调拨上架

### 3.23 Piece Rec 单件收货

### 3.24 Scan Verify

打包后分拣订单到对应Carrier



### 3.25 Carrier mamage 发货车管理

Carrier取货卡车管理



### 3.26 RMA Rec 

### 3.27 Bin Return





# 四、WMS PICKING APP

## 4.1 功能

仓库拣货流程。

分为Batch Picking，Putwall Picking两种拣货模式。

## 4.2 流程

Batch Picking流程：

创建拣货任务

绑定拣货Tote

扫描商品UPC

拣货有顺序，正序，逆序

界面上会显示订单是否加急，飞机标志

可上报问题库位，直接进入盘点任务列表



Putwall Picking流程：



Putwall Picking拣货完成后，还需要使用Putwall Sorting Web进行分拣。

地址：http://putwall.yamibuy.net/

Putwall Sorting流程：



## 4.3 接口

com.yamibuy.wms.cloud.api.OrderPickRest

- /wms/orderPick/111/checkUser

- /wms/orderPick/createPickingTask

- /wms/orderPick/bindToteNumber

- /wms/general/checkItemUPC

- /wms/orderPick/adjustLocation

- /wms/orderPick/checkResult

- /wms/orderPick/batchInsertProcessLog

- /wms/orderPick/outOfStock

- /wms/orderPick/pickingTaskFinish

com.yamibuy.wms.cloud.api.PutwallRest

- /wms/putwall/wall/{wallId}
- /wms/scan/{wallId}/{barcode}
- /wms/confirm/{wallId}/{slotId}/{in_user}





# 五、PACKING 打包

## 5.1 功能

打包出库。

网址：http://packing.yamibuy.net/

使用Packing打包网页版需要配合qz-tray工具使用，此工具有两个功能：

1. 检测获取本机局域网IP
2. 连接打印机打印出库相关Label



包装箱推荐

调用Carrier API创建Shipment

更新物流信息

更新预计送达时间



发送出库邮件，发送缺发邮件

## 5.2 流程

QC

打包



商品数量不足转补单，

订单已取消转PST Tote，

## 5.3 接口

com.yamibuy.wms.cloud.api.PackingRest

- /wms/packing/queryPackingInfo
- /wms/packing/packingOrder



# 六、PO

采购流程：

PR创建

PR审核

PO

PO确认



仓库收货后更新PO商品平均成本

​	更新到IM商品管理系统中



PO付款计划

PO成本核对



PR状态枚举

PO状态枚举



实体：

PM

​	BU

Vendor

PMA

Acc



## 6.1 新建采购单

同采购任务入口中的临时采购。

必选条件：

1. 供应商
2. PM
3. 送货仓库
4. 收货仓库

可选参数：

1. 分类
2. SKU / Goods ID
3. 商品名称

可调整参数：

1. 到货周期
2. 订货周期
3. 淡旺季转换系数
4. 安全库存天数
5. 分网修正参数 - 已废弃

界面功能：

1. 添加到采购车
2. 查看采购车
3. 上传采购单
4. 导出采购单
5. 显示隐藏列

采购车页面功能：

1. 新增采购商品
2. 删除采购商品

展示逻辑：

​	销量信息 - 销量数据来源

​	推荐采购数量

路由：poCart

## 6.2 采购任务

采购入口。

可选择PM - 必选，供应商，收货仓库，类别，采购日期。

采购模式：

1. 日常采购
2. 紧急采购 - 未实现
3. 临时采购

路由：purchaseTask

## 6.3 PR管理

采购单创建成功后，生成PR单

PR详情页面：

PR # 生成规则

预计到货日期

最迟到货日期

配送方式

采购备注

运费 税款 其他费用



页面功能：

显示隐藏列

下载PR Excel

PR审核

​	一审

​	二审

PR审核参数

PR审核权限

路由：prList

## 6.4 PO管理

PR审核通过后，生成PO单

两种方式：

1. 系统自动聚合
2. 手工选择PR创建PO



![image-20191226134054549](C:\Users\charles.kou\AppData\Roaming\Typora\typora-user-images\image-20191226134054549.png)



PO详情页面

PO # 生成规则

作废商品

调整商品ETA

调整商品收货容忍天数

调整商品折扣

采购确认

手工发送采购单至供应商

下载PO Excel

关闭PO

编辑采购确认

​	修改PO Vendor确认数量

查看商品收货批次，收货数量

PO成本核对

​	财务根据供应商发票Invoice核对PO成本

路由：newPoList

## 6.5 单品采购列表

查看商品采购数量

路由：poQuery

## 6.6 供应商管理

路由：vendorManagement

供应商信息

供应商简称

账期

供应商对应PM In Charge

供应商采购单发送时间

供应商联系邮箱

供应商淡旺季转换系数

收货仓参数

​	采购初始日期

​	订购周期

​	日常采购日

​	RMA Rate

​	头爆中尾待定商品安全库存天数

送货仓参数

​	到货周期

供应商启用停用



## 6.7 供应商商品

路由：itemMapping

供应商

​	商品SKU

​	供应商商品货号

​	送货仓库

​	商品箱规

​	商品标准成本

供应商商品更新

删除供应商商品



商品标准成本变更发送邮件通知

相关PM及二审权限人员

二审权限人员名单：julia.kang；nancy.lu；kyle.yao；sally.jia



## 6.8 PM商品管理

PM管控商品：有效状态的PM管控商品，仓库收货上架后，不会直接售卖，会存入保留库存中

路由：pmProduct

## 6.9 收货挂起

收货中图片/重量不符的商品

路由：wmsItemPending

TODO 废弃功能

## 6.10 付款计划查询

PO付款计划

路由：paymentPlanQuery

## 6.11 标准成本追踪

供应商商品成本变更日志

相关功能：供应商商品

路由：costUpdateTrack



## 6.12 数据表

yamibuy_po

https://docs.google.com/spreadsheets/d/1YxzcAYqQK-LkqWjU98JW64rzIVDn3Ec-mM8GWf0FjoY/edit#gid=83600504

![image-20191226152049767](C:\Users\charles.kou\AppData\Roaming\Typora\typora-user-images\image-20191226152049767.png)

## 6.13 外部接口提供

### 6.13.1 平均成本查询

IM商品管理调用查询商品平均成本



## 6.14 外部接口调用

### 6.14.1 平均成本更新

调用IM商品管理接口



# 七、INVENTORY

EC-Inventory

为网站、APP等客户端提供商品库存信息查询接口、下单库存扣减接口

Redis库存扣减 -> RabbitMq异步通知Central-Inventory -> Central-Inventory接收消息操作数据库扣减库存



Central-Inventory

为Central集群内部服务提供商品库存信息查询及调整接口

可用库存变更会同步到Redis中



库存 - 仓库数据基础表：inventory_warehouse

商品创建时库存初始化

仓库收货时更新最后一次到货时间



## 7.1 分仓管理

### 7.1.1 功能

查询数据库中商品库存信息；PM保留库存

### 7.1.2 路由

inventoryBranch

### 7.1.3 接口



### 7.1.4 数据表





## 7.2 库存日志

功能：查看商品库存变更日志；大数据量

路由：inventoryLog

## 7.3 新建调拨单

功能：调拨单创建，选择商品加入到调拨车中，提交生成调拨单

路由：tsCartAdd

## 7.4 调拨单管理

功能：调拨单管理

路由：tsOrderList

## 7.5 调拨manifest

功能：查看调拨载货清单

路由：tsManifest

## 7.6 可用库存监控

功能：可用库存Redis与数据库差异监控

路由：inventoryDiff

## 7.7 转换单管理

功能：商品组合销售，增加释放库存

路由：conversionList

## 7.8 虚库库存



## 7.9 接口

下单

订单出库

第三方商家库存调整

秒杀活动库存调整

采购

收货

上架

仓库损益

仓库通用工具调整

仓库调拨

​	调拨预占

​	调拨拣货

​	调拨QC

​	调拨出库

​	调拨收货

​	调拨上架



## 7.10 数据库SP

TODO 整理规划





# 八、Finance

财务Accounting部门会使用此功能导出销售报表与商家进行结算

销售汇总中，导出销售记录功能需求与SellerPortal中销售汇总功能同步

销售汇总中，销售记录，退货记录列表会使用Feign调用RMA系统接口





# 九、JOB

订单预占

生成补货任务

​	Stock -> Bin

调整补货任务优先级

生成Batch

更新订单预计送达时间及物流配送状态

生成盘点任务

生成保质期任务

库位库存快照

仓库Backlog统计

PR聚合生成PO

​	发送PO详细信息邮件到Vendor供应商

到货通知

商品库存变化

​	商品库存从无到有，从有到无，通知ES更新

FBY商品存储费用计算

商家账单结算

# 十、订单流程



首页 - 分类页 - 搜索页 - 活动页

​	cms

​	es

商品页

​	预计送达时间

​	库存

​	到货通知

购物车

​	库存

​	优惠券

结算

​	用户地址 - 地址校验

下单

​	支付

​	库存扣减

取消订单

​	库存归还

​	仓库停止拣货

​	退款

订单列表

订单详情

​	预计送达时间

​	订单状态

Down单

​	订单系统 -> 仓库系统

预占

​	仓库库位

​	订单配送方式（shipping_method carrier）预分配

​	生成订单预计送达时间

Batch

​	拣货批次

拣货

​	拣货任务

​	拣货车

​	拣货筐

分拣

打包

​	打包台

​	扫枪

​	打包箱

​	Label

​	Carrier 配送方式

订单配送

​	实际送达时间更新

​	配送状态更新



# 十一、 参考

WMS项目组主要模块Github仓库：https://yamibuy.atlassian.net/wiki/spaces/Dev/pages/81657868




# TODO
库位填充率：补货任务生成相关，商品配置相关，库位Size相关

订单延误程度

UPC重贴 功能弃用



电商系统演化



必选

Item

- Inventory

- SO
	- Payment

  



可选

WMS

Ship



依赖其他项目：

IM

SO



绩效统计

仓库员工拣货，打包绩效统计及页面展示

stock

用于UPS World Ship发货更新订单tracking number

TODO 计划废弃





权限相关

​	查看

​	分配



RMA系统逻辑

​	取消订单接口，订单停止拣货接口

