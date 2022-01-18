# 连接规范

---

## 协议URL（jdbc:db://）
 > 通过定义规范协议是连接规范的第一步
 ```java
         for (DriverInfo aDriver : registeredDrivers) {
            // If the caller does not have permission to load the driver then
            // skip it.
            if (isDriverAllowed(aDriver.driver, callerCL)) {
                try {
                    println("    trying " + aDriver.driver.getClass().getName());
                    Connection con = aDriver.driver.connect(url, info);
                    if (con != null) {
                        // Success!
                    println("getConnection returning " + aDriver.driver.getClass().getName());
                    return (con);
                }
            } catch (SQLException ex) {
                if (reason == null) {
                    reason = ex;
                }
            }

        } else {
            println("    skipping: " + aDriver.getClass().getName());
        }

    }
 ```
 > 其中以PG的实现为例，对URL进行判断 以区别不同的driver，是的环境中存在多种driver是可以直接通过规范的jdbcURL协议进行自动连接到正确的driver
 ```java
    
    if (!url.startsWith("jdbc:postgresql:")) {
      return null;
    }
 ```