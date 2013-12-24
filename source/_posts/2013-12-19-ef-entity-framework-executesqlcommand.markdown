---
layout: post
title: "EF使用ExecuteSqlCommand执行存储过程"
date: 2013-12-19 20:55:09 +0800
comments: true
categories: [EF,ado.net]
---
###Entity Framework使用ExecuteSqlCommand调用有输出参数存储过程
```csharp
var p1 = new SqlParameter("as_csocode", cSocode);
var p2 = new SqlParameter("ad_billdate", DateTime.Parse(billdate));
var p3 = new SqlParameter("as_cmaker", cMaker);
var p4 = new SqlParameter("as_cperson", PObuyerNo);
var p5Out = new SqlParameter("as_cpoid", System.Data.SqlDbType.NVarChar,200);
p5Out.Direction = System.Data.ParameterDirection.Output;
edmx.Database.ExecuteSqlCommand("exec dbo.Pr_po @as_csocode,@ad_billdate,@as_cmaker,@as_cperson,@as_cpoid output", p1, p2, p3, p4, p5Out);
returnStr = p5Out.Value.ToString();
```