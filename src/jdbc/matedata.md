# 通用MateData
---

## db的mateData

 ![常见接口](https://cdn.jsdelivr.net/gh/realcpf/img@main/book/jdbc/resultmatedata.png)


## 示例
 ```java
	try(Connection conn = DriverManager.getConnection(DRIVER_URL,USER_NAME,PASS_WD);
			PreparedStatement pst = conn.prepareStatement(sql)){
		pst.execute();
		try(ResultSet rs = pst.getResultSet()){
			ResultSetMetaData mData = rs.getMetaData();
			int count = mData.getColumnCount();
			for (int i = 1; i <= count; i++) {
				String catalogName = mData.getCatalogName(i);
				String className = mData.getColumnClassName(i);
				Integer displaySize = mData.getColumnDisplaySize(i);
				String columnLabel = mData.getColumnLabel(i);
				String cloumnLabel = mData.getColumnName(i);
				Integer columnType = mData.getColumnType(i);
				String columnTypeName = mData.getColumnTypeName(i);
				Integer precision = mData.getPrecision(i);
				Integer scale = mData.getScale(i);
				String scheamName = mData.getSchemaName(i);
				String tableName = mData.getTableName(i);
				System.out.printf(
						"%s.%s.%s type %d (%s) display size %d precision %d scale %d "
						+ "columnClass %s column label %s catalogName %s\n", 
						scheamName,tableName,columnLabel,columnType,columnTypeName,displaySize,
						precision,scale,className,columnLabel,catalogName);
			}
		}
	}
 ```