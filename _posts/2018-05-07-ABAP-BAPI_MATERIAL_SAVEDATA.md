---
layout: post
title: BAPI_MATERIAL_SAVEDATA函数相关
key: 20180507
tags: ABAP SAP MM
---

BAPI_MATERIAL_SAVEDATA

<!--more-->

可以用此BAPI创建新物料和修改已存在物料主数据。当创建物料时，必须输入物料号(material number),物料类型(material type),Industry sector，同时也要输入物料描述(material description，参数为MATERIALDESCRIPTION)和描述语言(language)。

当要修改物料时，你只需输入物料号(material number)就可以了。

在header data（必传的参数）中，至少要选定一个物料数据视图来创建，依据选定的视图，必须维护其他的参数，如果每个视图中必须的field没有维护，FM执行会返回错误，物料创建不会成功。

所有要维护的物料视图所需的数据，都要在调用此BAPI前在调用程序中填好相应的参数值，并且要打上操作标记，这样数据才能被FM维护到数据库中。如参数CLIENTDATA，其field的操作标记要维护到参数CLIENTDATAX中。有关联的操作标记checkbox table 的 物料视图数据table中不包括：语言相关文本数据（MAKT,MLTX），International Article Numbers (MEAN), 税的分类（MLAN），这些物料数据可直接传入相应的参数来生成。

如果内表或structure（参数）中含有度量单位（如CLIENTDATA-BASE_UOM），语言标识（如MATERIALDESCRIPTION-LANGU），或者是国家标识(如TAXCLASSIFICATIONS-DEPCOUNTRY)，此参数总会有一个以_ISO结尾的同名field。这就使得度量单位、语言标识、国家标识等我们可用标准的SAP code,也可以ISO 标准code。在未来业务流程中每个ISO code都有对应的标准SAP code。

如果要维护物料长文本（如：basic data texts, internal comments, purchase order texts, material memos, or sales texts）或自定义的物料数据field,一些特定的条件必须要定好，它们在参数MATERIALLONGTEXT 和 EXTENSIONIN中描述。

如果是要维护物料主数据的分类视图，要在创建完物料后接着调用BAPI: BAPI_OBJCL_CREATE 分类视图的创建

参数：
详情请参见BAPI的定义，很容易使用的。

另外：
对于BAPI的操作都要用BAPI_TRANSACTION_COMMIT来提交的，所以要判断BAPI的执行情况的返回值（参数RETURN），如果有错误要用BAPI_TRANSACTION_ROLLBACK取消所做的操作。建议提交BAPI操作时，加上wait参数，这样会减少某些错误。
```
call function ‘BAPI_TRANSACTION_COMMIT’
          exporting
            wait = ‘X’.
```