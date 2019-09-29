# 在Edge上添加并上线应用

Edge作为一个边缘计算平台，具备应用管理的能力。你可以在Edge平台上添加应用。这样一来，你选择的应用就可以读写Edge上所有资产的相关数据，并调用Edge平台的API。

## 任务描述

本文介绍了如何通过Edge的应用管理功能，为Edge平台添加应用，并上线应用的步骤。

## 开始前准备

- 你需要有Edge管理操作权限，如果没有请联系组织管理员添加，参见[策略，角色，与权限](/docs/iam/zh_CN/2.0.9/access_policy)。
- 你已经在EnOS上注册了应用，参见[管理应用](/docs/app-development/zh_CN/2.0.9/managing_apps)。

## 步骤

1. 选择 **EnOS Edge > Edge管理** ，点击需要添加应用的Edge设备实例的 **操作** 栏的 **查看** 。

 .. image:: ../../media/adding_app_1.png

2. 点击 **应用管理** 标签页，点击 **添加** 。

 .. image:: ../../media/adding_app_2.png

3. 从列表中列举的注册在本OU的所有尚未被添加到本Edge的应用中，勾选出需要添加到本Edge实例上的一个或多个应用，点击 **确定** 。

  已经被添加到其他Edge的应用仍可以重复添加到当前Edge。

  此时指定应用已经添加到了本Edge实例中。

 .. image:: ../../media/adding_app_3.png

4. 点击 **操作** 栏的 |online| 按钮并二次确认，让相应的应用获得读写该Edge实例上所有资产信息和API的权限。

 .. |online| image:: ../../media/button_online.png


## 结果

你选择的应用在指定的Edge设备实例上上线，能够读写该Edge上所有设备资产的信息，并根据需要调用相关API。如果你需要在Edge上运行应用，你还需要将应用部署在Edge上。

.. image:: ../../media/adding_app_4.png

## 后续操作

- [管理Edge上的应用](managing_app)。

