# JDBC

## 간단한 JDBC 코드
```
public class DBManager {

    // db user password 로컬에 맞춰 변경
    private final String url = "jdbc:oracle:thin:@localhost:1521:XE";

    private final String user = "scott";
    private final String pass = "tiger";

    private final String driverName = "oracle.jdbc.driver.OracleDriver";

    private static DBManager instance = new DBManager();

    private DBManager() {
        try {
            Class.forName(driverName);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    public static DBManager getInstance() {
        return instance;
    }

    public Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url, user, pass);
    }

    // try-with-resources를 위한 코드
    public void close(AutoCloseable... closeables) {
        for (AutoCloseable c : closeables) {
            if (c != null) {
                try {
                    c.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```