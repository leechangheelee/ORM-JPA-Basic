## **객체지향 쿼리 언어1 - 기본 문법**
  * 기본 문법과 쿼리 API
    
    ![image](https://user-images.githubusercontent.com/79301439/174746256-e0951890-13e2-4047-95b6-a333be8a8f00.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/174746329-d61ee9f1-1770-404b-b784-064675955a74.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/174746387-dcdd4e20-4c56-42a3-9a47-3377d0fd2aec.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/174746454-272cc0da-b3f5-4e81-924e-3554db6d8bc6.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/174746506-5db69a00-6a0c-45fb-9d4d-654c85f7fe2b.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/174746547-1d1dd161-0ea8-4454-9865-5b0703c759e1.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/174746619-b0e03cfc-7cf8-418f-bf72-60d6882a4a05.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/174746731-e086eb65-7d8a-41df-a2b8-f2208322bed2.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/174746811-f0185c15-a9f4-4176-a37f-02286ccc1269.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/174746875-abdf18e1-d928-432f-97ff-ddf3232d720e.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/174746950-5f08a791-82c2-4cf4-b8df-43606ac7e0bb.png)
    
    ```java
    package jpql;

    import javax.persistence.Embeddable;

    @Embeddable
    public class Address {

        private String city;
        private String street;
        private String zipcode;

        public String getCity() {
            return city;
        }

        public void setCity(String city) {
            this.city = city;
        }

        public String getStreet() {
            return street;
        }

        public void setStreet(String street) {
            this.street = street;
        }

        public String getZipcode() {
            return zipcode;
        }

        public void setZipcode(String zipcode) {
            this.zipcode = zipcode;
        }
    }
    ```
    
    ```java
    package jpql;

    import javax.persistence.*;

    @Entity
    public class Member {

        @Id @GeneratedValue
        private Long id;
        private String username;
        private int age;

        @ManyToOne//(fetch = FetchType.LAZY)
        @JoinColumn(name = "TEAM_ID")
        private Team team;

        public Long getId() {
            return id;
        }

        public void setId(Long id) {
            this.id = id;
        }

        public String getUsername() {
            return username;
        }

        public void setUsername(String username) {
            this.username = username;
        }

        public int getAge() {
            return age;
        }

        public void setAge(int age) {
            this.age = age;
        }

        public Team getTeam() {
            return team;
        }

        public void setTeam(Team team) {
            this.team = team;
        }
    }
    ```
    
    ```java
    package jpql;

    import javax.persistence.*;

    @Entity
    @Table(name = "ORDERS")
    public class Order {

        @Id @GeneratedValue
        private Long id;
        private int orderAmount;

        @Embedded
        private Address address;

        @ManyToOne
        @JoinColumn(name = "PRODUCT_ID")
        private Product product;

        public Long getId() {
            return id;
        }

        public void setId(Long id) {
            this.id = id;
        }

        public int getOrderAmount() {
            return orderAmount;
        }

        public void setOrderAmount(int orderAmount) {
            this.orderAmount = orderAmount;
        }

        public Address getAddress() {
            return address;
        }

        public void setAddress(Address address) {
            this.address = address;
        }

        public Product getProduct() {
            return product;
        }

        public void setProduct(Product product) {
            this.product = product;
        }
    }
    ```
    
    ```java
    package jpql;

    import javax.persistence.Entity;
    import javax.persistence.GeneratedValue;
    import javax.persistence.Id;

    @Entity
    public class Product {

        @Id @GeneratedValue
        private Long id;
        private String name;
        private int price;
        private int stockAmount;

        public Long getId() {
            return id;
        }

        public void setId(Long id) {
            this.id = id;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public int getPrice() {
            return price;
        }

        public void setPrice(int price) {
            this.price = price;
        }

        public int getStockAmount() {
            return stockAmount;
        }

        public void setStockAmount(int stockAmount) {
            this.stockAmount = stockAmount;
        }
    }
    ```
    
    ```java
    package jpql;

    import javax.persistence.Entity;
    import javax.persistence.GeneratedValue;
    import javax.persistence.Id;
    import javax.persistence.OneToMany;
    import java.util.ArrayList;
    import java.util.List;

    @Entity
    public class Team {

        @Id @GeneratedValue
        private Long id;

        private String name;

        @OneToMany(mappedBy = "team")
        private List<Member> members = new ArrayList<>();

        public Long getId() {
            return id;
        }

        public void setId(Long id) {
            this.id = id;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

    }
    ```
    
    ```java
    package jpql;

    import javax.persistence.*;

    public class JpaMain {

        public static void main(String[] args) {
            EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

            EntityManager em = emf.createEntityManager();

            EntityTransaction tx = em.getTransaction();
            tx.begin();

            try {

                Member member = new Member();
                member.setUsername("member1");
                member.setAge(10);
                em.persist(member);

                Member result = em.createQuery("select m from Member m where m.username = :username", Member.class)
                        .setParameter("username", "member1")
                        .getSingleResult();

                System.out.println("result = " + result.getUsername());

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
