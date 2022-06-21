## **객체지향 쿼리 언어1 - 기본 문법**
  * 프로젝션(SELECT)
    
    ![image](https://user-images.githubusercontent.com/79301439/174762618-253ee04d-ada3-4bf0-88f1-b8d1f3687270.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/174762742-31e58be1-b18f-48cf-aed9-397e9b5530b4.png)
    
    ```java
    package jpql;

    public class MemberDTO {

        private String username;
        private int age;

        public MemberDTO(String username, int age) {
            this.username = username;
            this.age = age;
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

                Member member = new Member();
                member.setUsername("member1");
                member.setAge(10);
                em.persist(member);

                em.flush();
                em.clear();

                List<MemberDTO> result = em.createQuery("select new jpql.MemberDTO(m.username, m.age) from Member m", MemberDTO.class)
                        .getResultList();

                MemberDTO memberDTO = result.get(0);
                System.out.println("memberDTO.getUsername() = " + memberDTO.getUsername());
                System.out.println("memberDTO.getAge()) = " + memberDTO.getAge());

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
