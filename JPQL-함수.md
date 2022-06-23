## **객체지향 쿼리 언어1 - 기본 문법**
  * JPQL 함수
    
    ![image](https://user-images.githubusercontent.com/79301439/175265577-58851bba-cd24-4c2a-80fb-44ce50b25538.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/175265649-c7234662-a39f-46ee-ba1f-4b1cb27ab5ac.png)
    
    ```java
    package dialect;

    import org.hibernate.dialect.H2Dialect;
    import org.hibernate.dialect.function.StandardSQLFunction;
    import org.hibernate.type.StandardBasicTypes;

    public class MyH2Dialect extends H2Dialect {

        public MyH2Dialect() {
            registerFunction("group_concat", new StandardSQLFunction("group_concat", StandardBasicTypes.STRING));
        }
    }
    ```
    
    persistence.xml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <persistence version="2.2"
                 xmlns="http://xmlns.jcp.org/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd">
        <persistence-unit name="hello">
            <properties>
                <!-- 필수 속성 -->
                <property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/>
                <property name="javax.persistence.jdbc.user" value="sa"/>
                <property name="javax.persistence.jdbc.password" value=""/>
                <property name="javax.persistence.jdbc.url" value="jdbc:h2:tcp://localhost/~/test"/>
                <property name="hibernate.dialect" value="dialect.MyH2Dialect"/>

                <!-- 옵션 -->
                <property name="hibernate.show_sql" value="true"/>
                <property name="hibernate.format_sql" value="true"/>
                <property name="hibernate.use_sql_comments" value="true"/>
                <property name="hibernate.hbm2ddl.auto" value="create" />
            </properties>
        </persistence-unit>
    </persistence>
    ```
    
    ```java
    package jpql;

    import javax.persistence.*;
    import java.util.List;

    public class JpaMain {

        public static void main(String[] args) {
            EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

            EntityManager em = emf.createEntityManager();

            EntityTransaction tx = em.getTransaction();
            tx.begin();

            try {

                Member member1 = new Member();
                member1.setUsername("관리자1");
                em.persist(member1);

                Member member2 = new Member();
                member2.setUsername("관리자2");
                em.persist(member2);

                em.flush();
                em.clear();

    //            String query = "select " +
    //                                   "case when m.age <= 10 then '학생요금' " +
    //                                   "     when m.age >= 60 then '경로요금' " +
    //                                   "     else '일반요금' " +
    //                                   "end " +
    //                           "from Member m";

    //            String query = "select coalesce(m.username, '이름 없는 회원') as username " +
    //                    "from Member m ";

                //size는 컬렉션의 크기를 돌려줌
                //String query = "select size(t.members) from Team t";

                //컬렉션에서 위치 찾는건데 index 사용 권장x
                //String query = "select index(t.members) from Team t";


                String query = "select group_concat(m.username) from Member m";

                List<String> result = em.createQuery(query, String.class)
                        .getResultList();

                for (String s : result) {
                    System.out.println("s = " + s);
                }

                tx.commit();
            } catch(Exception e) {
                tx.rollback();
                e.printStackTrace();
            } finally {
                em.close();
            }

            emf.close();
        }

    }
    ```
