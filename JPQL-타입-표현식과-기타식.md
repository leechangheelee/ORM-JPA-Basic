## **객체지향 쿼리 언어1 - 기본 문법**
  * JPQL 타입 표현식과 기타식
    
    ![image](https://user-images.githubusercontent.com/79301439/175251668-6ac4ede3-f7e4-4cc5-917c-ac13bebbddf7.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/175251741-36bf6d5e-0ec6-4ccd-a362-e1cca83a90b0.png)
    
    ```java
    package jpql;

    import javax.persistence.*;

    @Entity
    public class Member {

        @Id @GeneratedValue
        private Long id;
        private String username;
        private int age;

        @ManyToOne(fetch = FetchType.LAZY)
        @JoinColumn(name = "TEAM_ID")
        private Team team;

        @Enumerated(EnumType.STRING)
        private MemberType type;

        public void changeTeam(Team team) {
            this.team = team;
            team.getMembers().add(this);
        }

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

        @Override
        public String toString() {
            return "Member{" +
                    "id=" + id +
                    ", username='" + username + '\'' +
                    ", age=" + age +
                    '}';
        }

        public MemberType getType() {
            return type;
        }

        public void setType(MemberType type) {
            this.type = type;
        }
    }
    ```
    
    ```java
    package jpql;

    public enum MemberType {
        ADMIN, USER
    }
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

                Team team = new Team();
                team.setName("teamA");
                em.persist(team);

                Member member = new Member();
                member.setUsername("teamA");
                member.setAge(10);
                member.setType(MemberType.ADMIN);

                member.setTeam(team);

                em.persist(member);

                em.flush();
                em.clear();

                String query = "select m.username, 'HELLO', true from Member m " +
                                "where m.type = :userType";
    //            String query = "select m.username, 'HELLO', true from Member m " +
    //                    "where m.username is not null";
    //            String query = "select m.username, 'HELLO', true from Member m " +
    //                    "where m.age between 0 and 10";

                List<Object[]> result = em.createQuery(query)
                        .setParameter("userType", MemberType.ADMIN)
                        .getResultList();

                for (Object[] objects : result) {
                    System.out.println("objects[0] = " + objects[0]);
                    System.out.println("objects[1] = " + objects[1]);
                    System.out.println("objects[2] = " + objects[2]);
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
    
    ```java
    package jpabook.jpashop;

    import jpabook.jpashop.domain.Book;
    import jpabook.jpashop.domain.Item;

    import javax.persistence.EntityManager;
    import javax.persistence.EntityManagerFactory;
    import javax.persistence.EntityTransaction;
    import javax.persistence.Persistence;

    public class JpaMain {

        public static void main(String[] args) {

            EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

            EntityManager em = emf.createEntityManager();

            EntityTransaction tx = em.getTransaction();
            tx.begin();

            try {

                Book book = new Book();
                book.setName("JPA");
                book.setAuthor("김영한");

                em.persist(book);

                em.createQuery("select i from Item i where type(i) = Book ", Item.class)
                                .getResultList();

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
