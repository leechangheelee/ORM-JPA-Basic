## **JPA 시작하기**
  * Hello JPA - 애플리케이션 개발
    
    ![image](https://user-images.githubusercontent.com/79301439/170855766-ac989d9a-dbe5-487f-910b-a01c02d2a4b9.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/170855773-64b760e8-355b-4101-b50c-a349929b79e0.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/170855780-a5c0fb74-513b-40e2-8e02-2de77f2833ba.png)
    
    ```java
    package hellojpa;

    import javax.persistence.Entity;
    import javax.persistence.Id;

    @Entity
    public class Member {

        @Id
        private Long id;
        private String name;

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
    
    ```sql
    create table Member ( 
        id bigint not null, 
        name varchar(255), 
        primary key (id) 
    );
    ```
    
    ![image](https://user-images.githubusercontent.com/79301439/170855809-326d9d25-a8d2-47cb-8f99-9a7f8659d7e2.png)
    
    ```java
    package hellojpa;

    import javax.persistence.EntityManager;
    import javax.persistence.EntityManagerFactory;
    import javax.persistence.EntityTransaction;
    import javax.persistence.Persistence;
    import java.util.List;

    public class JpaMain {

        public static void main(String[] args) {
            EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

            EntityManager em = emf.createEntityManager();

            EntityTransaction tx = em.getTransaction();
            tx.begin();

            try {
                //생성
                /*Member member = new Member();
                member.setId(2L);
                member.setName("HelloB");

                em.persist(member);
                */

                //삭제
                /*Member findMember = em.find(Member.class, 1L);
                em.remove(findMember);*/

                //조회
                /*Member findMember = em.find(Member.class, 1L);
                System.out.println("findMember.id = " + findMember.getId());
                System.out.println("findMember.name = " + findMember.getName());*/

                //수정
                Member findMember = em.find(Member.class, 1L);
                findMember.setName("HelloJPA");

                //JPQL
                List<Member> findMembers = em.createQuery("select m from Member as m", Member.class)
                        .setFirstResult(5)
                        .setMaxResults(8)
                        .getResultList();

                for (Member member : findMembers) {
                    System.out.println("member.name = " + member.getName());
                }

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
    
    ![image](https://user-images.githubusercontent.com/79301439/170855833-edae3215-a649-445f-ac6d-78892a73de96.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/170855838-2a3b3417-8c0e-4913-81c9-a8a48c5cb87a.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/170855850-58158209-6168-4d6a-aff9-6c54251d3e23.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/170855863-5a11787a-7b1e-4665-bcff-8e8bc0c947c5.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/170855872-d51de598-5e03-4a7e-94e4-dda77d96cfbf.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/170855884-9fb19be6-cc97-42d0-b172-4916ead28c61.png)
