<img src="https://blog.kakaocdn.net/dn/LoKFA/btqVdeDfCg2/A15uwPLqujJP3of0Kn4qFK/img.png" 
width="800" height="300">
<br/><br/><br/>
# :pencil: **JPA를 공부하며 학습한 내용들**

<hr>

* ## 영속성 컨텍스트
    + JPA는 ORM을 어떻게 진행을 할 것인지, 영속성 컨텍스트를 어떻게 관리할 것인지가 중점이 된다.
    + 쿼리가 들어오면 EMF에서 EM을 생성하여 요청을  처리하고, DB 커넥션을 사용해 처리한다.
    + em.persist(val)은 val을 db에 저장하는 것이 아닌 영속성 컨텍스트에 객체를 저장하는 것을 의미한다.
    + 엔티티는 아래와 같은 생명주기를 가진다.

            비영속(transient) - 영속(managed) - 준영속(detached) - 삭제(removed)

    + 비영속 : 객체를 생성한 상태  
       영속 : persist를 통해 EM에 저장된 상태  
    준영속 : 엔티티를 detach를 통해 영속성에서 분리  
    삭제 : remove를 통해 영속성 콘텍스트에서 삭제

    + 내부에 1차 캐시를 가지고 있어, 쿼리가 들어올 때, 캐시 데이터를 먼저 확인한다.  
    만약 데이터가 존재하지 않는다면, DB를 통해 조회한 뒤 캐시에 저장하게 된다.  
    하지만 흔히들 쿼리를 사용한 뒤, EM을 닫는 경우가 대다수이기 때문에 1차 캐시는 크게 성능 이점을  
    가져와주지는 않는다.  

    + 엔티티의 동일성을 보장한다, 이는 자바 컬렉션과 동일한 성질로, SQL과의 차별성을 보인다.

    + 쓰기 지연 저장소가 존재한다. em에서 가져온 transaction을 커밋할 때 까지, 쿼리는 실행되지 않고, 모아져있다.  
batch_size를 설정하여 버퍼링 비스무리한 기능을 구현할 수 있다.

    + JPA에서 Entity를 선언할 때에는, NoArgsConstructor를 하나 만들어 두자.

    + 변경 감지 (더티 체킹)  
      + Setter를 이용해 값을 바꿀 경우 데이터의 변경이 자동으로 이루어진다.
      + 데이터의 수정을 진행할 경우, 꼭 이 방법을 통해 진행해야한다.
      + 커밋을 하게되면, 영속성 엔티티의 데이터와 스냅샷을 비교하여 업데이터 쿼리가 쓰기 지연 저장소에  
      저장되어 가능한 일이다.

    + 플러시
      + 영속성 컨텍스트의 변경내용을 데이터베이스에 반영하는 것을 플러시라고 한다.

      + em.flush(), em.commit(), JPQL쿼리 실행 등을 통해 플러시를 진행할 수 있다.

      + flush가 선언되면 앞서 설명한 변경 감지를 진행하게 된다.

<hr>

* ## 엔티티 매핑
    + @Entity가 붙은 클래스는 JPA가 관리한다.
  
    + 기본 생성자가 필수로 요구되며, final, enum interface, inner 클래스는 사용이 금지된다.

    + name을 직접 줄 수 있으나, 기본값으로 클래스 이름이 기본적으로 지정된다.

    + @Table 에노테이션에 name을 주어, 쿼리에 들어갈 테이블명을 설정할 수 있다.

    + 엔티티의 속성에 @Column에서 name, unique, length, nullable 등의 조건을 부여할 수 있다.

    + 엔티티에 부여될 수 있는 에노테이션

          @Id : PK
            @GenerateValue를 사용하면 자동으로 할당할 수 있다.
            strategy = GenerationType.IDENTITY : 기본 키 생성을 데이터베이스에 맡김
            strategy = GenerationType.SEQUENCE : SequenceGenerator를 이용해 부여
              strategy = GenerationType.TABLE : TableGenerator를 이용해 부여


          @Column : 앞서 설명했듯이 테이블 생성에 추가적인 정보 부여
            name : 컬럼명
            insertable, updatable : 등록, 변경 가능 여부
            nullable : 널 값의 허용 여부
            unique : 유니크 제약 조건 부여 (유니크 조건은 테이블에 걸자)
            length : 문자 길이제약
            
          @Enumerated(EnumType.String) : EnumType 속성을 가지게 할 때,
          @Temporal(TemporalType.TIMESTAMP) : 날짜 정보, 그냥 LocalDate(Time) 쓰자
          @Lob : description 등 큰 정보들을 넣고 싶을 때 부여
          @Transient : DB와 상관 없는 속성을 부여하고 싶을 때.

<hr/>

* ## 연관관계 매핑
  * 테이블은 외래 키로 조인을 사용해서 연관된 테이블을 찾지만,  
  객체는 참조를 사용해서 연관된 객체를 찾는다.  
  연관관계를 매핑하지 않으면, 객체지향적인 설계에서 크게 벗어날 수 있다.
 
  * 단방향 연관관계
  
        * @ManyToOne, @OneToOne 어노테이션을 사용한다.
        * FK의 경우 @JoinColumn(name = FK명) 어노테이션을 추가로 기입한다.  

    
  * 양방향 연관 관계
  
        * mappedBy를 이용해 반대쪽의 참조를 알린다.
        * 둘 중 하나로 외래키를 관리해야하기 때문이다.
        * 객체의 두 관계 중 하나를 연관관계의 주인으로 지정한다.
        * 주인이 아닌 쪽은 읽기만 가능하며, 주인만이 외래키를 관리한다.
        * 주인은 mappedBy 속성을 사용하지 않는다.
        * 주인이 아니면 mappedBy 속성으로 주인을 지정한다.
        * 외래키가 있는 곳을 주인으로 지정한다.
        * 다수 쪽을 연관관계의 주인으로 지정한다.

  * 





  