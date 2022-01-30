# 事务与回滚


---

 ```java
 try(Connection conn = DriverManager.getConnection(DRIVER_URL,USER_NAME,PASS_WD)){
        PreparedStatement pst = conn.prepareStatement(sql);
        Savepoint sp = null;
        try {
            conn.setAutoCommit(false);
            sp = conn.setSavepoint("savepoint");
            pst.execute();
            conn.commit();
        } catch (Exception e) {
            try {
                if(sp == null){
                    conn.rollback();
                } else {
                    conn.rollback(sp);
                }
            } catch (Exception e1) {
                e1.printStackTrace();
            }
        
        }
	}
 ```