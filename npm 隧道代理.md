## 修改 npm 代理

打开 cmd 或 powershell，通过命令行为 npm 设置代理

|                 |                                                                                                                                             |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| 1  <br>2  <br>3 | # clash 默认的隧道地址为http://127.0.0.1:7890  <br>npm config set proxy http://127.0.0.1:7890  <br>npm config set https-proxy http://127.0.0.1:7890 |

如果要取消代理，通过

|          |                                                            |     |
| -------- | ---------------------------------------------------------- | --- |
| 1  <br>2 | npm config delete proxy  <br>npm config delete https-proxy |     |
```mermaid
classDiagram
    %% 产品类
    class Product {
        +ObjectId productId        %% 唯一标识产品
        +String productName        %% 产品名称
        +String stockUnit          %% 库存单位
        +String specification       %% 产品规格
        +ObjectId processRoute     %% 外键，引用工艺路线
        +String[] productImages    %% 产品图片URL数组
        +Number stockQuantity      %% 库存数量
        +String status             %% 产品状态（如“启用”、“禁用”、“停产”）
        +Number price              %% 产品价格
    }

    %% 工序类
    class Process {
        +ObjectId processId                       %% 唯一标识工序
        +String processName                       %% 工序名称
        +ObjectId[] reportingPermissions          %% 允许报工的部门ID列表，支持多个部门
        +String[] defectiveItems                  %% 不良品项列表
        +Object collectionData                    %% 工序采集的额外数据
    }

    %% 工艺路线类
    class ProcessRoute {
        +ObjectId routeId                         %% 唯一标识工艺路线
        +String routeName                         %% 工艺路线名称
        +ObjectId[] operationList                 %% 工序ID列表，顺序
        +String description                        %% 工艺路线描述
    }

    %% 产品工序单价类
    class ProductProcessPrice {
        +ObjectId id                              %% 唯一标识
        +ObjectId productId                       %% 外键，引用产品
        +ObjectId processId                       %% 外键，引用工序
        +Number unitPrice                         %% 工序单价
        +String pricingType                       %% 计费类型（"计时"或"计件"）
        +Number timeRate                          %% 计时单价（仅在计时类型时有效）
        +Number pieceRate                         %% 计件单价（仅在计件类型时有效）
    }

    %% 工单类
    class WorkOrder {
        +ObjectId orderId                         %% 唯一标识工单
        +String workOrderNumber                   %% 工单编号
        +String productionProgress                 %% 报工信息
        +String productionStatus                   %% 生产状态
        +ObjectId productInfo                     %% 外键，引用产品
        +Date creationDate                        %% 工单创建日期
        +Date completionDate                      %% 工单完成日期
        +Number totalPrice                        %% 工单总价格，基于产品和工序的价格计算
    }

    %% 用户类
    class User {
        +ObjectId userId                          %% 唯一标识用户
        +String name                              %% 用户姓名
        +String phoneNumber                       %% 用户电话
        +ObjectId departmentId                    %% 外键，引用部门
        +String role                              %% 用户角色
        +String status                            %% 用户状态（如“活跃”、“非活跃”）
    }

    %% 部门类
    class Department {
        +ObjectId departmentId                    %% 唯一标识部门
        +String departmentName                    %% 部门名称
        +ObjectId departmentHead                  %% 部门负责人
    }

    %% 报工信息类
    class WorkReporting {
        +ObjectId reportId                        %% 唯一标识报工信息
        +ObjectId reporter                        %% 报工人（用户ID）
        +ObjectId workOrderId                    %% 外键，引用工单
        +ObjectId processId                       %% 外键，引用工序
        +ObjectId productId                       %% 外键，引用产品
        +Number processUnitPrice                  %% 工序单价，基于产品获取
        +String pricingType                       %% 计费类型（"计时"或"计件"）
        +Number goodQuantity                      %% 良品数量
        +Number defectiveQuantity                 %% 不良品数量
        +Number workingHours                      %% 工作小时数（仅在计时时有效）
        +Number piecesCompleted                   %% 完成件数（仅在计件时有效）
        +Object additionalInfo                    %% 工序需要采集的额外信息
        +String auditStatus                       %% 审核状态（如“待审核”、“审核通过”、“审核不通过”）
        +ObjectId auditor                         %% 审核人（用户ID）
        +Date auditDate                          %% 审核日期
    }

    %% 关联关系
    Product --> ProcessRoute : processRoute
    ProcessRoute --> Process : operationList
    ProductProcessPrice --> Product : productId
    ProductProcessPrice --> Process : processId
    WorkOrder --> Product : productInfo
    WorkReporting --> WorkOrder : workOrderId
    WorkReporting --> Process : processId
    WorkReporting --> Product : productId
    WorkReporting --> User : reporter
    WorkReporting --> User : auditor
    User --> Department : departmentId
```



![[Pasted image 20250103111402.png]]