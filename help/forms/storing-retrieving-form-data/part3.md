---
title: 从MySQL数据库存储和检索表单数据
description: 多部分教程，指导您完成存储和检索表单数据所涉及的步骤
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 1%

---


# 创建OSGI服务以获取数据

编写以下代码以获取存储的自适应表单数据。 简单查询用于获取与给定GUID关联的自适应表单数据。 然后，所获取的数据被返回到调用应用程序。 在第一步中创建的同一数据源在此代码中被引用。


```java
package com.techmarketing.core.impl;

import com.techmarketing.core.AemFormsAndDB;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

@Component(
        service = AemFormsAndDB.class
)
public class AemformWithDB implements AemFormsAndDB {
    private final Logger log = LoggerFactory.getLogger(getClass());
   
    @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformstutorial))")
    private DataSource dataSource;

    @Override
    public String getData(String guid) {
        log.debug("### inside my getData of AemformWithDB");
        Connection con = getConnection();
        try {
            Statement st = con.createStatement();
            String query = "SELECT afdata FROM aemformstutorial.formdata where guid = '" + guid + "'" + "";
            log.debug(" The query is " + query);
            ResultSet rs = st.executeQuery(query);
            while (rs.next()) {
                return rs.getString("afdata");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return null;
    }

    public Connection getConnection() {
        log.debug("Getting Connection ");
        Connection con = null;
        try {
            con = dataSource.getConnection();
            log.debug("got connection");
            return con;
        } catch (Exception e) {
            log.debug("not able to get connection ");
            e.printStackTrace();
        }
        return null;
    }
}
```

## 接口

下面是使用的接口声明

```java
package com.techmarketing.core;

public interface AemFormsAndDB {
   public String getData(String guid);
}
```
