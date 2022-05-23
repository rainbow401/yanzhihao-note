# MybatisPlus

## 自定义sql 使用包装器以及分页器

```java
List<Map<String, Object>> projectInfoList(
  IPage<TbProjectinfoEntity> IPage, @Param("ew") Wrapper<TbProjectinfoEntity> wrapper
);
```



```sql
        SELECT
        p1.Id AS id,
        p1.ProjectName AS projectName,
        p1.ProjectAddr AS projectAddr,
        p1.TotalResidue AS totalResidue,
        p1.TotalEarthWork AS totalEarthWork,
        p1.TotalGravelSoil AS TotalGravelSoil,
        p1.FloorPrice AS floorPrice,
        p1.PriceRange AS priceRange,
        p1.EarnestMoney AS earnestMoney,
        p1.QuotationWay AS quotationWay,
        DATE_FORMAT(p1.AucStartTime,"%Y-%m-%d %H:%i:%S") AS aucStartTime,
        DATE_FORMAT(p1.AucEndTime,"%Y-%m-%d %H:%i:%S") AS aucEndTime,
        DATE_FORMAT(p1.ActuralEndTime,"%Y-%m-%d %H:%i:%S") AS acturalEndTime,
        p1.ProjectStatus AS projectStatus,
        p1.PaymentStatus AS paymentStatus,
        p1.CurrentPrice AS currentPrice,
        p1.RevokeReason AS revokeReason,
        p1.ProjectCode AS projectCode,
        p1.`DELAYED` AS `delayed`
        FROM
        tb_projectinfo p1
        ${ew.customSqlSegment}
```





## mybaits

```java
@AutomapConstructor
// 查询结果指定映射为java对象时的构造方法
```
