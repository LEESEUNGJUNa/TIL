#Today I Learn

오늘은 테스트 코드 작성하는 방법을 공부했다.

1.단위 테스트 - 
 단위테스트는 가장 작은단위를 하나씩 테스트 하는것이라고 한다.
엔티티 클래스 (DI주입을 안받는)를 테스트 하는 코드를 작성하는 것은 비교적 쉬운것같다.
하지만 엔티티를 주입받는 서비스단을 테스트하는 단위테스트는 가짜 레파지토리 역할을 하는 Mock Object를 사용해야하기때문에 조금까다로워진다.


```java
    private List<Product> products = new ArrayList<>();
    // 상품 테이블 ID: 1부터 시작
    private Long productId = 1L;

    // 상품 저장
    public Product save(Product product) {
        for (Product existProduct : products) {

            if (existProduct.getId().equals(product.getId())) {
                int myprice = product.getMyprice();
                existProduct.setMyprice(myprice);
                return existProduct;
            }
        }

```
위 코드는 테스트를 위해 만들어진 클래스중 일부인데 ProductRepository 가해주는 Save를 구현한것이다.  

위와같은 귀찮은 코드작성이 싫다면 Mockito 라는 프레임워크를 사용하면 조금은더 쉽게 테스트코드를 작성할수있다.
```java

    @Mock
    ProductRepository productRepository; // 이런식으로 레파지토리를 생성하고
            
     when(productRepository.findById(productId))
       .thenReturn(Optional.of(product));
       
    //findById 함수에 productId가 들어오면 product를 반환한다고 약속한후에

    Product result = productService.updateProduct(productId, requestMyPriceDto);
    //위와같이 작성하면 updateProduct 함수 내부에서 productRepository.findById 함수를 사용했을때
    // 약속한 값을 반환함으로서 테스트를 할수있다.

```

2. 통합 테스트 - 두개이상의 모듈을 합쳐서 테스트하는것이다 두개이상의 모듈 사이에 데이터를 주고받는 과정에서 문제가 있는지 없는지 판단할수 있다.
나는 스프링부트테스트로 테스트코드를 작성했는데 굉장히 편한것같다. 등록되어있는빈을 그대로 사용할수있고, 클래스명 위에 @Transactional을 붙이면
트랜잭션이 끝나면서 commit이아니라 rollback을 해주기때문에 데이터베이스에 테스트 데이터가 반영되지도 않는다.
```java
    //======================
    @Autowired
    ProductService productService;

    Long userId = 100L;
    Product createdProduct = null;
    int updatedMyPrice = -1;

    @Test
    @Order(1)
    @DisplayName("신규 관심상품 등록")
    void test1() {
// given
        String title = "Apple <b>에어팟</b> 2세대 유선충전 모델 (MV7N2KH/A)";
        String imageUrl = "https://shopping-phinf.pstatic.net/main_1862208/18622086330.20200831140839.jpg";
        String linkUrl = "https://search.shopping.naver.com/gate.nhn?id=18622086330";
        int lPrice = 77000;
        ProductRequestDto requestDto = new ProductRequestDto(
                title,
                imageUrl,
                linkUrl,
                lPrice
        );

// when
        Product product = productService.createProduct(requestDto, userId);

// then
        assertNotNull(product.getId());
        assertEquals(userId, product.getUserId());
        assertEquals(title, product.getTitle());
        assertEquals(imageUrl, product.getImage());
        assertEquals(linkUrl, product.getLink());
        assertEquals(lPrice, product.getLprice());
        assertEquals(0, product.getMyprice());
        createdProduct = product;
    }
    //======
```
위와같이 작성하면 된다.

3.mvc테스트 - 이건 공부하면서 받은 느낌은 controller를 단위테스트 한다는 생각이든다. 컨트롤러 내부에서 호출하는 Service가 MockObject처럼 동작하는것같다.
아무튼 HTTP 요청에 헤더나,바디를 설정하고 URL을 설정한후 그에맞는 결과값이 맞는지 확인하는 순으로 코드를 작성하면 된다.

느낀점 - 어려운것같다.


 