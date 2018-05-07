---
layout: post
title: CS_BOM_EXPL_MAT_V2函数相关
key: 20180507
tags: ABAP SAP MM
---

CS_BOM_EXPL_MAT_V2

<!--more-->

```
call function 'CS_BOM_EXPL_MAT_V2'
    exporting
       capid = 'PP01'           "???只能取PP01??
       datuv = itout-datuv   "??订单的下达日期???
       emeng = '1'              "需求数量?这个参数干啥用
       mtnrv = itout-matnr
       mehrs = ''               "多层展开，'X'表示是，''表示否
*        stpst = pm_stpst
*        stlal = pm_stlal
       stlan = '1'              "BOM的用途，1表示生产
       werks = itout-WERKS
    importing
       topmat = selpool
       dstst  = dstst_flg
    tables
       stb = stb[]
*         matcat = matcat
    exceptions
       material_not_found    = 4
       no_plant_data         = 8
       no_bom_found          = 12
       no_suitable_bom_found = 16
       alt_not_found         = 24
       missing_authorization = 28
       conversion_error      = 36
```

BOM是有“有效期”的，用CS12查询时输入不同的valid from,则得出的BOM结果就有可能不同。用FM：CS_BOM_EXPL_MAT_V2取BOM也是一样的道理。一般情况下，将以上的参数datuv 赋予当前日期sy-datum，即可得到当前最新的有效BOM。

对于capid参数，一般情况下，我们所取的都生产用BOM，所以必须指定为"PP01" 。如果是其它类型的BOM应用，则可以按需要选择：
- PP01------ Production - general
- BEST------ Inventory management
- INST ------ Plant maintenance
- PC01 ------ Costing
- PI01 ------ Process manufacturing
- SD01------ Sales and distribution