## **JPA 시작하기**
  * Hello JPA - 프로젝트 생성
    
    ![image](https://user-images.githubusercontent.com/79301439/170853306-f2f90846-34fc-47f2-a6a4-31f1a7f1a467.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/170853315-3edcc6d7-5722-4060-8f85-185c6abbebda.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/170853317-9f7cc5bb-ebbf-466b-83de-e20aa7890650.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/170853322-401ff975-9e75-4e15-9016-3b303b3c33b5.png)
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>

        <groupId>jpa-basic</groupId>
        <artifactId>ex1-hello-jpa</artifactId>
        <version>1.0.0</version>

        <properties>
            <maven.compiler.source>8</maven.compiler.source>
            <maven.compiler.target>8</maven.compiler.target>
        </properties>

        <dependencies>
            <!-- JPA 하이버네이트 -->
            <dependency>
                <groupId>org.hibernate</groupId>
                <artifactId>hibernate-entitymanager</artifactId>
                <version>5.3.10.Final</version>
            </dependency>
            <!-- H2 데이터베이스 -->
            <dependency>
                <groupId>com.h2database</groupId>
                <artifactId>h2</artifactId>
                <version>2.1.212</version>
            </dependency>
        </dependencies>

    </project>
    ```
    
    ![image](https://user-images.githubusercontent.com/79301439/170853345-abe2789a-0709-4e9c-9ac8-2c7732b0b49d.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/170853357-06dabd05-f98b-4957-8be5-3bc5bb6a56b5.png)
    
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
                <property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/>

                <!-- 옵션 -->
                <property name="hibernate.show_sql" value="true"/>
                <property name="hibernate.format_sql" value="true"/>
                <property name="hibernate.use_sql_comments" value="true"/>
                <!--<property name="hibernate.hbm2ddl.auto" value="create" />-->
            </properties>
        </persistence-unit>
    </persistence>
    ```
    
    ![image](https://user-images.githubusercontent.com/79301439/170853374-95436195-3cfa-4d67-ba16-bc7f296cb8b3.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/170853380-feec5c3c-ed70-4c09-b2e7-d9a5247791c8.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/170853386-3fa1b922-447d-4124-80f4-d3e3f8021fa9.png)
