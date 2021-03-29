---
title: "[.NET Core] JsonDocument 與 DataTable 的互相轉換"
date: 2020-11-16T06:19:00+08:00
lastmod: 2020-11-16T06:19:00+08:00
draft: true
tags: ["Host", "dotNetCore"]
categories: ["NET Core 3.1"]
author: "tigernaxo"

autoCollapseToc: true
---
Stream 讀取的資料會作為 JsonElement 寫到 JsonDocument 的 RootElement 屬性當中，
```c#
public JsonElement jsonFromDataTable(DataTable dt) {
    using (var stream = new MemoryStream()) {
        using (var writer = new Utf8JsonWriter(stream)) {
            writer.WriteStartArray(); // 起始一個 Json Array
            foreach (DataRow row in dt.Rows) {
                writer.WriteStartObject();
                foreach (DataColumn column in row.Table.Columns) {
                    writer.WritePropertyName(column.ColumnName);
                    if (row[column.ColumnName] == DBNull.Value)
                        writer.WriteNullValue();
                    else
                        JsonSerializer.Serialize(writer, row[column]);
                }
                writer.WriteEndObject();
            }
            writer.WriteEndArray();
        }
        return JsonDocument.Parse( stream.ToArray() ).RootElement;
    }
}
```
使用方式：
```c#
```

寫成擴充方法好像更直覺一點：
```c#
namespace Extensions
{
  public static class DataTableExtensions
  {
    public static JsonElement toJArray(this DataTable dt) 
    {
      using (var stream = new MemoryStream()) 
      {
        using (var writer = new Utf8JsonWriter(stream)) 
        {
          writer.WriteStartArray(); // 起始一個 Json Array
          foreach (DataRow row in dt.Rows) 
          {
            writer.WriteStartObject(); // 起始一個 Json Object
            foreach (DataColumn column in row.Table.Columns) 
            {
              // 設定 Json Object 的屬性名稱
              writer.WritePropertyName(column.ColumnName); 
              // 如果是 Null 的話就寫入 null，否寫入 String
              if (row[column.ColumnName] == DBNull.Value)
                writer.WriteNullValue();
              else
                JsonSerializer.Serialize(writer, row[column]);
            }
            writer.WriteEndObject();
          }
          writer.WriteEndArray();
        }
        return JsonDocument.Parse( stream.ToArray() ).RootElement;
      }
    }
  }
}
```