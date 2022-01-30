# Statement不只是语句

---

## 语句能力
 > 提供基础的语句执行能力，包括但不限于返回结果，判断超时，发送取消，批执行
 ```java
    Statement statement = conn.createStatement();
    statement.addBatch(sql);
    statement.cancel();
    statement.clearBatch();
    statement.execute(sql);
    statement.executeQuery(sql);
    statement.executeUpdate(sql, 1);
    statement.setQueryTimeout(1);
 ```

## 游标能力
 ```java
    statement = conn.createStatement(ResultSet.TYPE_SCROLL_INSENSITIVE, ResultSet.CONCUR_UPDATABLE);
    statement.setCursorName("cursor");
    statement.setFetchDirection(ResultSet.FETCH_FORWARD);
    ResultSet rSet = statement.executeQuery(sql);
    while (rSet.next()) {
        // 卷动、更新数据
        rSet.absolute(2);
        rSet.afterLast();
        rSet.beforeFirst();
        rSet.cancelRowUpdates();
        rSet.deleteRow();
        rSet.insertRow();
        rSet.moveToInsertRow();
        rSet.updateInt(columnLabel, x);
        rSet.updateRow();
        rSet.rowDeleted()
    }
 ```