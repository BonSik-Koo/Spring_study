๐ RestAPI ๊ฐ๋ฐ ๊ธฐ๋ณธ
==========================
* ํ์ ๋ฑ๋ก API
    <details>
    <summary>@PostMapping("/api/v2/members")</summary>

    ![image](https://user-images.githubusercontent.com/96917871/180197438-79daef42-bd34-4aa4-99cd-b217430a904d.png)
    </details>

* ํ์ ์์  API
    <details>
    <summary>@PutMapping("/api/v2/members/{id}")</summary>

    ![image](https://user-images.githubusercontent.com/96917871/180197737-b1fdab09-b30d-42b7-a80b-c6ac13be72f3.png)
    </details>

* ํ์ ์กฐํ API
    <details>
    <summary>@GetMapping("/api/v2/members")</summary>

    ![image](https://user-images.githubusercontent.com/96917871/180197913-f2e1b4df-f513-4ca3-ab46-96333fcde8df.png)
    </details>
    
    
๐ RestAPI ๊ฐ๋ฐ ๊ณ ๊ธ
====================================================================
โ  ์ง์ฐ๋ก๋ฉ๊ณผ ์กฐํ ์ฑ๋ฅ ์ต์ ํ("xToOne(ManyToOne, OneToOne)" ๊ด๊ณ ์ต์ ํ)
-----------------------------------------------------------------------------

* ์ฃผ๋ฌธ ์กฐํ V1 - ์ํฐํฐ๋ฅผ ์กฐํํด์ DTO๋ก ๋ฐํ(fetch join ์ฌ์ฉx)
  > ์ง์ฐ์ฐ๋ก๋ฉ์ผ๋ก ์ฟผ๋ฆฌ N๋ฒ ํธ์ถ(DTO ๋ฅผ ์์ฑํ ๋ ์ง์ฐ๋ก๋ฉ์ผ๋ก๋ ๊ฐ์ฒด์ ๊ฐ์ ์ฌ์ฉํ๋ ์์ ์์ ์ฟผ๋ฆฌ๋ฌธ ํธ์ถ) !! => "N+1"๋ฌธ์  ์ฌ์ ํ ์กด์ฌ
  > 
  > JPQL ๊ฒฐ๊ณผ๋ก order ๊ฐ 2๊ฐ ๋ฐํ => 1(JPQL) + order(member(2) + delivery(2)) => "์ด 5๋ฒ ์ฟผ๋ฆฌ๋ฌธ ๋ฐ์" (N+1๋ฌธ์  ๋ฐ์)!!!
   <details>
    <summary> @GetMapping("/api/v2/simple-orders")</summary>

    ![image](https://user-images.githubusercontent.com/96917871/180198940-9a30f4eb-3d08-42d4-91cb-f3a3051d8848.png)
    </details>
    
* ์ฃผ๋ฌธ ์กฐํ V2 - ์ํฐํฐ๋ฅผ ์กฐํํด์ DTO ๋ก ๋ณํ(fetch join ์ฌ์ฉO)
  > fetch join ์ผ๋ก ์ฟผ๋ฆฌ 1๋ฒ ํธ์ถ(ํ์ํ ๋ฐ์ดํฐ๋ค์ ์๋๊น ํด๋น ๋ฐ์ดํฐ๋ค๋ง ๋ฝ๋ "join fetch" ์ฟผ๋ฆฌ๋ฌธ ์์ฑํด์ ์ฌ์ฉํ๊ธฐ => "N+1" ๋ฌธ์  ํด๊ฒฐ
   <details>
    <summary> @GetMapping("/api/v3/simple-orders")</summary>
    
    ```
     public List<Order> findAllWithMemberDelivery() {
        String sql = "select o from Order o join fetch o.member m join fetch o.delivery d";
        List<Order> result = em.createQuery(sql, Order.class)
                .getResultList();
        return result;
    }
    ```
    
    ![image](https://user-images.githubusercontent.com/96917871/180200730-a8d109f4-cad0-4e9c-b3b0-c143489ebeb8.png)
    </details>
    
* ์ฃผ๋ฌธ ์กฐํ V3 - ์ํฐํฐ๋ฅผ ์กฐํํด์ JPA ๊ฒฐ๊ณผ๋ก DTO๊ฐ์ฒด ๋ฐ๋ก ๋ณํ
  > * ์ฟผ๋ฆฌ 1๋ฒ ํธ์ถ
  > * "select ์ ์์ ์ํ๋ ๋ฐ์ดํฐ๋ง ์ ํํด์ ์กฐํ"
  > 
  > -> API ์คํ์ ์ต์ ํ๋ ๋ ํฌ์งํ ๋ฆฌ ๊ธฐ๋ฅ์ ํ๊ฒ ๋๋ค.! -> ํจํค์ง๋ฅผ ๋ฐ๋ก ๋ง๋๋๊ฒ์ด ์ข๋ค.!
  > 
  > => "ํจ์น์กฐ์ธ"์ผ๋ก ํด๊ฒฐ๋์ง ์์ "์ฑ๋ฅ์ต์ ํ"๋ฅผ ์กฐ๊ธ ํด๊ฒฐํ ์ ์๋ค.!
   <details>
    <summary> @GetMapping("/api/v4/simple-orders")</summary>
    
    ```
    public List<OrderSimpleQueryDto> findOrderDtos() {
        String sql = "select new jpabook.jpashop.repository.order.simpleQuery.OrderSimpleQueryDto(o.id, m.name, o.orderDate, o.status, d.address)" +
                " from Order o join o.member m join o.delivery d";

        return em.createQuery(sql, OrderSimpleQueryDto.class)
                .getResultList();
    }
    ```
    
    ![image](https://user-images.githubusercontent.com/96917871/180200839-dba36eb7-b353-4e8c-913e-ffe4c0eb8bed.png)
    </details>

โก ์ปฌ๋ ์ ์กฐํ ์ฑ๋ฅ ์ต์ ํ("OneToMany" ๊ด๊ณ ์ต์ ํ)
-----------------------------------------------------------------------------
* ์ฃผ๋ฌธ ์กฐํ V1 - ์ํฐํฐ๋ฅผ ์กฐํํด์ DTO ๋ก ๋ณํ(fetch join ์ฌ์ฉX)
  > ํธ๋์ญ์ ์์์ ์ง์ฐ ๋ก๋ฉ ํ์ -> "N+1" ๋ฌธ์  ๋ฐ์!
   <details>
    <summary>@GetMapping("/api/v2/orders")</summary>
    
    ```
    {
    "orders": [
        {
            "orderId": 1,
            "name": "useA",
            "orderDate": "2022-07-21T19:53:13.231747",
            "orderStatus": "ORDER",
            "address": {
                "city": "์์ธ",
                "street": "1",
                "zipcode": "111"
            },
            "orderItems": [
                {
                    "itemName": "JPA1 BOOK",
                    "orderPrice": 10000,
                    "count": 1
                },
                {
                    "itemName": "JPA2 BOOK",
                    "orderPrice": 20000,
                    "count": 2
                }
            ]
        },
        {
            "orderId": 2,
            "name": "userB",
            "orderDate": "2022-07-21T19:53:13.309542",
            "orderStatus": "ORDER",
            "address": {
                "city": "์ง์ฃผ",
                "street": "2",
                "zipcode": "2222"
            },
            "orderItems": [
                {
                    "itemName": "SPRING1 BOOK",
                    "orderPrice": 20000,
                    "count": 3
                },
                {
                    "itemName": "SPRING2 BOOK",
                    "orderPrice": 40000,
                    "count": 4
                }
            ]
        }
      ],
      "count": 2
    }
    ```
    </details>
    
* ์ฃผ๋ฌธ ์กฐํ V2 - ์ํฐํฐ๋ฅผ ์กฐํํด์ DTO ๋ก ๋ณํ(fetch join ์ฌ์ฉ o)
  > * "ํ์น์กฐ์ธ"์ผ๋ก SQL๋ฌธ์ด ํ๋ฒ๋ง ์คํ๋จ
  > * distinct๋ฅผ ์ฌ์ฉํ ์ด์ ๋ "1:N"์กฐ์ธ์ด๋ฏ๋ก DB์ ๊ฒฐ๊ณผ์ row๊ฐ ์ฆ๊ฐํ๋ค. ๊ทธ ๊ฒฐ๊ณผ order์ํฐํฐ๋ 2๊ฐ์ง๋ง orderItem์ด 4๊ฐ ์ด๋ฏ๋ก 4๊ฐ๊ฐ ๋๋ค.
  > * **JPA์ distinct๋ SQL์ distinct๋ฅผ ์ถ๊ฐํ๊ณ , ๋ํด์ ๊ฐ์ ์ํฐํฐ๊ฐ ์กฐํ๋๋ฉด, ์ ํ๋ฆฌ์ผ์ด์์์ ์ค๋ณต์ ๊ฑธ๋ฌ์ค๋ค. ์ด ์์์ order๊ฐ ์ปฌ๋ ์ ํ์น ์กฐ์ธ ๋๋ฌธ์ ์ค๋ณต ์กฐํ ๋๋ ๊ฒ์ ๋ง์์ค๋ค**
  > * **ํ์ด์ง ๊ธฐ๋ฅ์ ์ฌ์ฉํ ์ ์๋ค.!!** , ์ปฌ๋ ์ ํ์น์กฐ์ธ์ ์ฌ์ฉํ๋ฉด ํ์ด์ง์ด ๋ถ๊ฐ๋ฅํ๋ค(๋ฐ์ดํฐ๊ฐ ๋ปฅํ๊ธฐ ๋๋). ํ์ด๋ฒ๋ค์ดํธ๋ ๊ฒฝ๋ก ๋ก๊ทธ๋ฅผ ๋จ๊ธฐ๊ณ  ๋ชจ๋  ๋ฐ์ดํฐ๋ฅผ DB์ ์ฝ๊ณ  ๋ฉ๋ชจ๋ฆฌ์ฐจ์์์ ํ์ด์ง์ ํ๋ค.(๋งค์ฐ ์ํ)
  
   <details>
    <summary>@GetMapping("/api/v3/orders")</summary>
    
    ```
    public List<Order> findAllWithItem() {
        /**
         * distinct ๊ธฐ๋ฅ
         * 1. ์ฟผ๋ฆฌ๋ฌธ์ ์ง์  ๋ฃ์ด์ค๋ค. -> DB์ ์์ฅ์์๋ ๋ชจ๋  ๋ฐ์ดํฐ๊ฐ ๊ฐ์์ผ ์ค๋ณต์ ์์ ์ฃผ๋๋ฐ ํด๋น ๊ฒฝ์ฐ์๋ order์ ํ๋๊ฐ ๊ฐ๊ณ  orderItem์ด ๋ค๋ฅด๋๊น ์ค๋ณต์ ๊ฑฐ๊ฐ ์๋๋ค.
         * 2. JPA๊ฐ ์ฟผ๋ฆฌ์ ๊ฒฐ๊ณผ๋ก "Order"์ ๊ฐ์ฒด์์ ๊ฐ์ id(Order์ id)๊ฐ ๊ฐ์๊ฑด ๋ ๋ ค์ค๋ค. -> ์ค๋ณต ์ ๊ฑฐ๊ฐ ๋๋ค.!!
         */
        String sql = "select distinct o from Order o join fetch o.member m join fetch o.delivery d join fetch o.orderItems oi join fetch oi.item i";
        return em.createQuery(sql, Order.class)
                .getResultList();
    }
    ```
    
   ```
   {
    "orders": [
        {
            "orderId": 1,
            "name": "useA",
            "orderDate": "2022-07-21T19:53:13.231747",
            "orderStatus": "ORDER",
            "address": {
                "city": "์์ธ",
                "street": "1",
                "zipcode": "111"
            },
            "orderItems": [
                {
                    "itemName": "JPA1 BOOK",
                    "orderPrice": 10000,
                    "count": 1
                },
                {
                    "itemName": "JPA2 BOOK",
                    "orderPrice": 20000,
                    "count": 2
                }
            ]
        },
        {
            "orderId": 2,
            "name": "userB",
            "orderDate": "2022-07-21T19:53:13.309542",
            "orderStatus": "ORDER",
            "address": {
                "city": "์ง์ฃผ",
                "street": "2",
                "zipcode": "2222"
            },
            "orderItems": [
                {
                    "itemName": "SPRING1 BOOK",
                    "orderPrice": 20000,
                    "count": 3
                },
                {
                    "itemName": "SPRING2 BOOK",
                    "orderPrice": 40000,
                    "count": 4
                }
            ]
        }
      ],
     "count": 2
   }
   
   ```
    </details>


* ์ฃผ๋ฌธ ์กฐํ V3 - ์ปฌ๋์ ์กฐ์ธ์ ํ์ด์ง ํ๊ณ ๊ทน๋ณต
  * ์ปฌ๋ ์์ ํ์น ์กฐ์ธํ๋ฉด ํ์ด์ง์ด ๋ถ๊ฐ๋ฅํ๋ค.
    > ์ผ๋ค๋์์ ์ผ(1)์ ๊ธฐ์ค์ผ๋ก ํ์ด์ง์ ํ๋ ๊ฒ์ด ๋ชฉ์ ์ด๋ค. ๊ทธ๋ฐ๋ฐ ๋ฐ์ดํฐ๋ ๋ค(N)๋ฅผ ๊ธฐ์ค์ผ๋ก row๊ฐ ์์ฑ๋๋ค
  * ์ด ๊ฒฝ์ฐ ํ์ด๋ฒ๋ค์ดํธ๋ ๊ฒฝ๊ณ  ๋ก๊ทธ๋ฅผ ๋จ๊ธฐ๊ณ  ๋ชจ๋  DB ๋ฐ์ดํฐ๋ฅผ ์ฝ์ด์ ๋ฉ๋ชจ๋ฆฌ์์ ํ์ด์ง์ ์๋ํ๋ค. ์ต์์ ๊ฒฝ์ฐ ์ฅ์ ๋ก ์ด์ด์ง ์ ์๋ค.
  * **ํด๊ฒฐ๋ฒโ**
    > 1. ๋จผ์  ToOne(OneToOne, ManyToOne) ๊ด๊ณ๋ฅผ ๋ชจ๋ ํ์น์กฐ์ธ ํ๋ค. ToOne ๊ด๊ณ๋ row์๋ฅผ ์ฆ๊ฐ์ํค์ง ์์ผ๋ฏ๋ก ํ์ด์ง ์ฟผ๋ฆฌ์ ์ํฅ์ ์ฃผ์ง ์์
    > 2. ์ปฌ๋ ์์ ์ง์ฐ ๋ก๋ฉ์ผ๋ก ์กฐํํ๋ค.(ํจ์น์กฐ์ธํ์ง ๋ง๊ธฐ!)
    > 3. **์ง์ฐ ๋ก๋ฉ ์ฑ๋ฅ ์ต์ ํ๋ฅผ ์ํด hibernate.default_batch_fetch_size , @BatchSize ๋ฅผ ์ ์ฉํ๋ค.** , ์ด ์ต์์ ์ฌ์ฉํ๋ฉด ์ปฌ๋ ์์ด๋, ํ๋ก์ ๊ฐ์ฒด๋ฅผ ํ๊บผ๋ฒ์ ์ค์ ํ size ๋งํผ IN ์ฟผ๋ฆฌ๋ก ์กฐํํ๋ค.
    > ![image](https://user-images.githubusercontent.com/96917871/180218800-64395b0f-1714-45be-be5a-a4a4f2d9aac5.png)
  
   * **๊ฒฐ๊ณผโ**
     > * ์ฟผ๋ฆฌ ํธ์ถ์๊ฐ "1+N"์์ "1+1"๋ก ์ต์ ํ ๋๋ค. -> 1+N๋ฌธ์ ๋ฅผ ์์ ํ ํด๊ฒฐํ๋๊ฒ ์๋๋ค ์ต์ ํ๋ฅผ 
     > * ์ปฌ๋ ์ ํ์น ์กฐ์ธ์ ํ์ด์ง์ด ๋ถ๊ฐ๋ฅํ์ง๋ง ์ด ๋ฐฉ๋ฒ์ ๊ฐ๋ฅํ๋ค.
       
   <details>
    <summary>@GetMapping("/api/v3.1/orders")</summary>
    
    ```
    public List<Order> findAllWithMemberDelivery(int offset, int limit) { //ํ์ด์ง!!!
        String sql = "select o from Order o join fetch o.member m join fetch o.delivery d";
        List<Order> result = em.createQuery(sql, Order.class)
                .setFirstResult(offset)
                .setMaxResults(limit)
                .getResultList();
        return result;
    }
    ```
    
    ```
    {
    "orders": [
        {
            "orderId": 1,
            "name": "useA",
            "orderDate": "2022-07-21T19:53:13.231747",
            "orderStatus": "ORDER",
            "address": {
                "city": "์์ธ",
                "street": "1",
                "zipcode": "111"
            },
            "orderItems": [
                {
                    "itemName": "JPA1 BOOK",
                    "orderPrice": 10000,
                    "count": 1
                },
                {
                    "itemName": "JPA2 BOOK",
                    "orderPrice": 20000,
                    "count": 2
                }
            ]
        },
        {
            "orderId": 2,
            "name": "userB",
            "orderDate": "2022-07-21T19:53:13.309542",
            "orderStatus": "ORDER",
            "address": {
                "city": "์ง์ฃผ",
                "street": "2",
                "zipcode": "2222"
            },
            "orderItems": [
                {
                    "itemName": "SPRING1 BOOK",
                    "orderPrice": 20000,
                    "count": 3
                },
                {
                    "itemName": "SPRING2 BOOK",
                    "orderPrice": 40000,
                    "count": 4
                }
            ]
        }
      ],
      "count": 2
    }
    ```
    </details>


* ์ฃผ๋ฌธ ์กฐํ V4 - JPA์์ DTO๋ก ๋ฐ๋ก ์กฐํ, ์ปฌ๋์ N ์กฐํ(1+N์ฟผ๋ฆฌ, ์ปฌ๋ ์์ ์ ์ธํ ์ฐ๊ด๊ด๊ณ๋ ํ๋ฒ์ ์กฐํํ ์ปฌ๋ ์ ๊ฐ์N ๋งํผ) , ํ์ด์ง ๊ฐ๋ฅ
  * <details>
    <summary>Dto</summary>
       
    ```
    public class OrderItemQueryDto {

        @JsonIgnore
        private Long id; //V5์์ ์ง์  map ๋งคํํ ๋ ์ฌ์ฉ

        private String itemName;
        private int orderPrice;
        private int count;

        public OrderItemQueryDto(Long id, String itemName, int orderPrice, int count) {
         this.id = id;
         this.itemName = itemName;
         this.orderPrice = orderPrice;
         this.count = count;
        }
    } 
    ```
    ```
    public class OrderQueryDto {

        private Long orderId;
        private String name; //์ฃผ๋ฌธ ๊ณ ๊ฐ ์ด๋ฆ
        private LocalDateTime orderDate; //์ฃผ๋ฌธ ์๊ฐ
        private OrderStatus orderStatus;
        private Address address;
         private List<OrderItemQueryDto> orderItems;

        public OrderQueryDto(Long orderId, String name, LocalDateTime orderDate, OrderStatus orderStatus, Address address) {
        this.orderId = orderId;
        this.name = name;
        this.orderDate = orderDate;
        this.orderStatus = orderStatus;
        this.address = address;
         }
    } 
    ```
    </details>
       
   * Repository
     > 1. 1:N(์ปฌ๋ ์์ ์ ์ธํ) ๊ด๊ณ๋ฅผ join์ ํตํด ๋ฐ๋ก Dto๊ฐ์ฒด๋ก ์์ฑ
     > 2. 1:N ๊ด๊ณ์ธ orderItem์ item๊ณผ join์ ํตํด ๊ฐ์ ธ์จ๋ค.
     > order๊ฐ 2๊ฐ์ด๊ณ (๊ฐ ๋ค๋ฅธ๊ณ ๊ฐ) ๊ฐ order๋ง๋ค orderitem์ด 2๊ฐ์ด๊ธฐ ๋๋ฌธ์ "์ปฌ๋ ์ 2๋ฒ ์ฟผ๋ฆฌ๋ฌธ ๋ฐ์
     *  <details>
        <summary>repository</summary>
       
        ```
        /**
        * ์ปฌ๋ ์์ ๋ณ๋๋ก ์กฐํ
        * Query: ๋ฃจํธ 1๋ฒ, ์ปฌ๋ ์ N ๋ฒ ์ฟผ๋ฆฌ๋ฌธ์ด ์คํ๋๋ค.!!
        * <๋จ๊ฑด ์กฐํ์์ ๋ง์ด ์ฌ์ฉํ๋ ๋ฐฉ์></๋จ๊ฑด>
        */
        public List<OrderQueryDto> findOrderQueryDtos() {
            List<OrderQueryDto> result = findOrders();
            result.stream()
                    .forEach(o -> {
                        List<OrderItemQueryDto> orderItem = findOrderItem(o.getOrderId()); // ๋ฃจํ๋ฅผ ๋๋ฉฐ "orderItem"์ ์ฌ๋ฌ๊ฐ๋๊น ์ง์  ๋ฃ์ด์ค๋ค.
                        o.setOrderItems(orderItem);
                    } );
            return result;
        }
        /**
        * 1:N ๊ด๊ณ์ธ orderItems ์กฐํ
        */
        private List<OrderItemQueryDto> findOrderItem(Long orderId) {
            String sql = "select new jpabook.jpashop.repository.order.query.OrderItemQueryDto(oi.order.id,i.name, oi.orderPrice, oi.count)" +
                    " from OrderItem oi " +
                    " join oi.item i" +
                    " where oi.order.id = :orderId";

          return em.createQuery(sql, OrderItemQueryDto.class)
                  .setParameter("orderId", orderId)
                  .getResultList();
        }
    
        /**
         * 1:N ๊ด๊ณ(์ปฌ๋ ์)๋ฅผ ์ ์ธํ ๋๋จธ์ง๋ฅผ ํ๋ฒ์ ์กฐํ
        */
        public List<OrderQueryDto> findOrders() { //1:N ๊ด๊ณ์ธ "OrderItem" ๋ฆฌ์คํธ์ด๋ฏ๋ก ๋ฐ๋ก ์์ฑ์(OrderQueryDto)์ ๋ฃ์์ ์๋ค.
             String sql = "select new jpabook.jpashop.repository.order.query.OrderQueryDto(o.id,m.name,o.orderDate,o.status,d.address)" +
                " from Order o join o.member m join o.delivery d";

             return em.createQuery(sql, OrderQueryDto.class)
                .getResultList();
             }
        ```
        </details>
       
    * Http Reponse body
      
      <details>
      <summary>@GetMapping("/api/v4/orders")</summary>
          
      ```
      {
        "orders": [
            {
            "orderId": 1,
            "name": "useA",
            "orderDate": "2022-07-21T19:53:13.231747",
            "orderStatus": "ORDER",
            "address": {
                "city": "์์ธ",
                "street": "1",
                "zipcode": "111"
            },
            "orderItems": [
                {
                    "itemName": "JPA1 BOOK",
                    "orderPrice": 10000,
                    "count": 1
                },
                {
                    "itemName": "JPA2 BOOK",
                    "orderPrice": 20000,
                    "count": 2
                }
            ]
          },
         {
            "orderId": 2,
            "name": "userB",
            "orderDate": "2022-07-21T19:53:13.309542",
            "orderStatus": "ORDER",
            "address": {
                "city": "์ง์ฃผ",
                "street": "2",
                "zipcode": "2222"
            },
            "orderItems": [
                {
                    "itemName": "SPRING1 BOOK",
                    "orderPrice": 20000,
                    "count": 3
                },
                {
                    "itemName": "SPRING2 BOOK",
                    "orderPrice": 40000,
                    "count": 4
                }
            ]
         }
         ],
         "count": 2
         }
      ```
      </details>
       

       
       
