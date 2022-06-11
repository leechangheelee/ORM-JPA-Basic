## **고급 매핑**
  * Mapped Superclass - 매핑 정보 상속
    
    ![image](https://user-images.githubusercontent.com/79301439/173174437-16494e20-d01a-49a0-bcef-28e7ac499800.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/173174444-b08189f2-9c90-4fe2-a474-d98d1484663e.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/173174462-a69e8a67-eece-45d4-b7e5-77036c5dfb04.png)
    
    ```java
    package hellojpa;

    import javax.persistence.Column;
    import javax.persistence.MappedSuperclass;
    import java.time.LocalDateTime;

    @MappedSuperclass
    public abstract class BaseEntity {

        @Column(name = "INSERT_MEMBER")
        private String createdBy;
        private LocalDateTime createdDate;
        @Column(name = "UPDATE_MEMBER")
        private String lastModifiedBy;
        private LocalDateTime lastModifiedDate;

        public String getCreatedBy() {
            return createdBy;
        }

        public void setCreatedBy(String createdBy) {
            this.createdBy = createdBy;
        }

        public LocalDateTime getCreatedDate() {
            return createdDate;
        }

        public void setCreatedDate(LocalDateTime createdDate) {
            this.createdDate = createdDate;
        }

        public String getLastModifiedBy() {
            return lastModifiedBy;
        }

        public void setLastModifiedBy(String lastModifiedBy) {
            this.lastModifiedBy = lastModifiedBy;
        }

        public LocalDateTime getLastModifiedDate() {
            return lastModifiedDate;
        }

        public void setLastModifiedDate(LocalDateTime lastModifiedDate) {
            this.lastModifiedDate = lastModifiedDate;
        }
    }
    ```
    
    ```java
    package hellojpa;

    import javax.persistence.*;
    import java.util.ArrayList;
    import java.util.List;

    @Entity
    public class Team extends BaseEntity {

        @Id @GeneratedValue
        @Column(name = "TEAM_ID")
        private Long id;
        private String name;

        @OneToMany
        @JoinColumn(name = "TEAM_ID")
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

        public List<Member> getMembers() {
            return members;
        }
    }
    ```
    
    ```java
    package hellojpa;

    import javax.persistence.*;
    import java.util.ArrayList;
    import java.util.List;

    @Entity
    public class Member extends BaseEntity {

        @Id @GeneratedValue
        @Column(name = "MEMBER_ID")
        private Long id;

        @Column(name = "USERNAME")
        private String username;

        @ManyToOne
        @JoinColumn(name = "TEAM_ID", insertable = false, updatable = false)
        private Team team;

        @OneToOne
        @JoinColumn(name = "LOCKER_ID")
        private Locker locker;

        @OneToMany(mappedBy = "member")
        private List<MemberProduct> memberProducts = new ArrayList<>();

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
    }
    ```
    
    ```java
    package hellojpa;

    import javax.persistence.EntityManager;
    import javax.persistence.EntityManagerFactory;
    import javax.persistence.EntityTransaction;
    import javax.persistence.Persistence;
    import java.time.LocalDateTime;

    public class JpaMain {

        public static void main(String[] args) {
            EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

            EntityManager em = emf.createEntityManager();

            EntityTransaction tx = em.getTransaction();
            tx.begin();

            try {

                Member member = new Member();
                member.setUsername("user1");
                member.setCreatedBy("lee");
                member.setCreatedDate(LocalDateTime.now());

                em.persist(member);

                em.flush();
                em.clear();

                tx.commit();
            } catch(Exception e) {
                tx.rollback();
            } finally {
                em.close();
            }

            emf.close();
        }
    }
    ```
