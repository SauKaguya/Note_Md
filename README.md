import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
import java.sql.ResultSet;

public class Main {
    public static void main(String[] args) {
        String sql = "select * from nation limit 1";
        String url = "jdbc:mysql://localhost:3306/test?serverTimezone=UTC";
        String username = "root";
        String password = "0101sghy@LJ";
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            Connection connection = DriverManager.getConnection(url, username, password);
            Statement st = connection.createStatement();
            ResultSet rs = st.executeQuery(sql);
            rs.next();
            String name = rs.getString(2);
           // String sex = rs.getString(2);
           // int age = rs.getInt(3);
            System.out.println(name);
           // System.out.println(name + " " + sex + " " + age);
            connection.close();
        } catch (SQLException e) {
            e.getStackTrace();
        } catch (ClassNotFoundException e) {
            e.getStackTrace();
        }
    }
}
