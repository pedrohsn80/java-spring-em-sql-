# java-spring-em-sql-
código básico em java com framework spring em banco de dados sql
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

@SpringBootApplication
@RestController
public class SpringSQLApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringSQLApplication.class, args);
    }

    @GetMapping("/")
    public String getDataFromDatabase() {
        String url = "jdbc:mysql://localhost:3306/postgreSQL";
        String username = "pedrohsn80";
        String password = "123456789";

        try {
            Connection connection = DriverManager.getConnection(url, username, password);
            Statement statement = connection.createStatement();
            ResultSet resultSet = statement.executeQuery("SELECT * FROM tabela");

            StringBuilder result = new StringBuilder();
            while (resultSet.next()) {
                String column1 = resultSet.getString("coluna1");
                String column2 = resultSet.getString("coluna2");
                result.append("Coluna 1: ").append(column1).append(", Coluna 2: ").append(column2).append("<br>");
            }

            resultSet.close();
            statement.close();
            connection.close();

            return result.toString();
        } catch (Exception e) {
            e.printStackTrace();
            return "Erro ao conectar ao banco de dados.";
        }
    }
}
