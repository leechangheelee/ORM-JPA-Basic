## **프록시와 연관관계 관리**
  * 영속성 전이(CASCADE)와 고아 객체
    
    ![image](https://user-images.githubusercontent.com/79301439/174031681-2d1df86a-4c97-44d6-81e7-7ca6c0b6a216.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/174031777-ecb43b6b-b037-4175-875b-324a999030bf.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/174031906-2ec77527-56e9-4e8b-b569-f595b7ce1b7b.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/174031957-8b51eb4c-668f-4813-a93f-54d6ba59387c.png)
    
  * 고아 객체
    
    ![image](https://user-images.githubusercontent.com/79301439/174032089-2c980c18-ff07-4531-a92f-bc1f820d6722.png)
    
    ![image](https://user-images.githubusercontent.com/79301439/174032185-715a2865-abe7-4534-94eb-2226bea18434.png)
    
  * 영속성 전이 + 고아 객체, 생명 주기
    
    ![image](https://user-images.githubusercontent.com/79301439/174032296-3aab8c9d-8f77-41fe-921a-15016ca23f92.png)
    
    ```java
    package hellojpa;

    import javax.persistence.*;
    import java.util.ArrayList;
    import java.util.List;

    @Entity
    public class Parent {

        @Id @GeneratedValue
        private Long id;

        private String name;

        @OneToMany(mappedBy = "parent", cascade = CascadeType.ALL, orphanRemoval = true)
        //@OneToMany(mappedBy = "parent", orphanRemoval = true)
        //@OneToMany(mappedBy = "parent", cascade = CascadeType.ALL)
        private List<Child> childList = new ArrayList<>();

        public void addChild(Child child) {
            childList.add(child);
            child.setParent(this);
        }

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

        public List<Child> getChildList() {
            return childList;
        }

    }
    ```
    
    ```java
    package hellojpa;

    import javax.persistence.*;

    @Entity
    public class Child {

        @Id @GeneratedValue
        private Long id;

        private String name;

        @ManyToOne
        @JoinColumn(name = "parent_id")
        private Parent parent;

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

        public Parent getParent() {
            return parent;
        }

        public void setParent(Parent parent) {
            this.parent = parent;
        }
    }
    ```
    
    ```java
    package hellojpa;

    import javax.persistence.*;

    public class JpaMain {

        public static void main(String[] args) {
            EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

            EntityManager em = emf.createEntityManager();

            EntityTransaction tx = em.getTransaction();
            tx.begin();

            try {

                Child child1 = new Child();
                Child child2 = new Child();

                Parent parent = new Parent();
                parent.addChild(child1);
                parent.addChild(child2);

                em.persist(parent);

                em.flush();
                em.clear();

                Parent findParent = em.find(Parent.class, parent.getId());
                findParent.getChildList().remove(0);

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
