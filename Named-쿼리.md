## **객체지향 쿼리 언어2 - 중급 문법**
  * Named 쿼리
    
    ![image](https://user-images.githubusercontent.com/79301439/175758282-0bd7beb4-962a-494f-ab1e-f77df5854f67.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/175758289-0b61d2fe-d58f-4f33-8224-28eb59b34f01.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/175758297-e0e772fc-ade8-472b-86a1-14a2843aa126.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/175758302-f0db8da2-7634-4cce-a942-f0515f63fb77.png)
    
    ```java
    package jpql;

    import javax.persistence.*;

    @Entity
    @NamedQuery(
            name = "Member.findByUsername",
            query = "select m from Member m where m.username = :username"
    )
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

    import javax.persistence.*;
    import java.util.List;

    public class JpaMain {

        public static void main(String[] args) {
            EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

            EntityManager em = emf.createEntityManager();

            EntityTransaction tx = em.getTransaction();
            tx.begin();

            try {

                Team teamA = new Team();
                teamA.setName("팀A");
                em.persist(teamA);

                Team teamB = new Team();
                teamB.setName("팀B");
                em.persist(teamB);

                Member member1 = new Member();
                member1.setUsername("회원1");
                member1.setTeam(teamA);
                em.persist(member1);

                Member member2 = new Member();
                member2.setUsername("회원2");
                member2.setTeam(teamA);
                em.persist(member2);

                Member member3 = new Member();
                member3.setUsername("회원3");
                member3.setTeam(teamB);
                em.persist(member3);

                em.flush();
                em.clear();

                List<Member> resultList = em.createNamedQuery("Member.findByUsername", Member.class)
                        .setParameter("username", "회원1")
                        .getResultList();

                for (Member member : resultList) {
                    System.out.println("member = " + member);
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
