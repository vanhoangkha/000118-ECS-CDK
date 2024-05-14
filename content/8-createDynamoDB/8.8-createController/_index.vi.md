---
title : "Tạo controller"
date :  "`r Sys.Date()`" 
weight : 8
chapter : false
pre : " <b> 8.8 </b> "
---

#### Tạo productDTO
1. Tạo một thư mục tên **dto** và một file mới có tên là **ProductDto**

![Architect](/images/8/controller/01.png/?featherlight=false&width=60pc)

2. Tạo ProductDTO như sau

```java
public record ProductDto(
    String id, String name, String code, float price, String model,
    @JsonInclude(JsonInclude.Include.NON_NULL)
    String url
) {
    public ProductDto(Product product) {
        this(product.getId(), product.getProductName(), product.getCode(),
                product.getPrice(), product.getModel(), product.getProductUrl());
    }

    static public Product toProduct(ProductDto productDto) {
        Product product = new Product();
        product.setId(product.getId());
        product.setProductName(productDto.name);
        product.setCode(productDto.code());
        product.setModel(productDto.model());
        product.setPrice(productDto.price());
        product.setProductUrl(productDto.url());
        return product;
    }
}
```
![Architect](/images/8/controller/02.png/?featherlight=false&width=60pc)


#### Tạo controller

3. Trong class ProductsController khởi tạo productsRepository
+ Khai báo **productsRepository** Là một dependency injection thông qua constructor, được sử dụng để truy xuất dữ liệu từ các sản phẩm trong cơ sở dữ liệu
+ Khởi tạo **productsRepository** trong contructor sử dụng @Autowired, cho phép Spring tự động inject productsRepository vào controller.

```java
private final ProductsRepository productsRepository;

    @Autowired
    public ProductsController(ProductsRepository productsRepository) {
        this.productsRepository = productsRepository;
    }
```
![Architect](/images/8/controller/03.png/?featherlight=false&width=60pc)

4. Tạo phương thức **getAllProducts** .Phương thức này được đánh dấu bởi @GetMapping, chỉ xử lý các HTTP GET request tới đường dẫn được chỉ định (/products). Phương thức này bao gồm
   + Ghi log thông tin "Get all products".
   + Tạo một danh sách rỗng productsDto để lưu trữ các đối tượng ProductDto.
   + Sử dụng productsRepository.getAll().items().subscribe(...) để lấy danh sách sản phẩm từ repository.
   + Đối với mỗi product được lấy ra từ repository, nó tạo một ProductDto tương ứng và thêm vào productsDto
   + Cuối cùng, phương thức trả về một **ResponseEntity<List<ProductDto>>** chứa danh sách productsDto đã tạo, với mã trạng thái **HttpStatus.OK**.

```java
@GetMapping
    public ResponseEntity<List<ProductDto>> getAllProducts() {
        LOG.info("Get all products");
        List<ProductDto> productsDto = new ArrayList<>();

        productsRepository.getAll().items().subscribe(product -> {
            productsDto.add(new ProductDto(product));
        }).join();

        return new ResponseEntity<>(productsDto, HttpStatus.OK);
    }
```
![Architect](/images/8/controller/04.png/?featherlight=false&width=60pc)

5. Tạo phương thức **getProductById** Phương thức này được đánh dấu bởi **@GetMapping("{id}")**, có nghĩa là nó sẽ xử lý các HTTP GET request tới "/products/{id}", trong đó {id} là một path variable
+ Phương thức này lấy id từ đường dẫn và sử dụng **productsRepository.getById(id).join()** để đợi và lấy ra Product từ repository
+ Nếu product khác null, nó trả về ProductDto của product đó với mã trạng thái **HttpStatus.OK**.
+ Nếu product là null, nó trả về thông báo lỗi "Product not found" với mã trạng thái **HttpStatus.NOT_FOUND**.

```java
@GetMapping("{id}")
    public ResponseEntity<?> getProductById(@PathVariable("id") String id) {
        Product product = productsRepository.getById(id).join();
        if (product != null) {
           return new ResponseEntity<>(new ProductDto(product), HttpStatus.OK);
        } else {
            return new ResponseEntity<>( "Product not found", HttpStatus.NOT_FOUND);
        }
    }
```
![Architect](/images/8/controller/05.png/?featherlight=false&width=60pc)

6. Tạo phương thức **createProduct**. Phương thức này được đánh dấu bởi **@PostMapping**, xử lý các HTTP POST request tới "/products".
+ Phương thức này tạo một Product từ ProductDto được gửi lên (productDto), gán một id mới bằng UUID, sau đó lưu **productCreated** vào **productsRepository** bằng **productsRepository.create(productCreated).join()**.
+ Sau khi tạo sản phẩm thành công, nó ghi log thông tin về sản phẩm được tạo và trả về ProductDto của sản phẩm đó với mã trạng thái **HttpStatus.CREATED**.

```java
@PostMapping
    public ResponseEntity<ProductDto> createProduct(@RequestBody ProductDto productDto) {
        Product productCreated = ProductDto.toProduct(productDto);
        productCreated.setId(UUID.randomUUID().toString());
        productsRepository.create(productCreated).join();

        LOG.info("Product created - ID: {}", productCreated.getId());
        return new ResponseEntity<>(new ProductDto(productCreated), HttpStatus.CREATED);
    }
```
![Architect](/images/8/controller/06.png/?featherlight=false&width=60pc)

7. Tạo phương thức **deleteProductById**. Phương thức này được đánh dấu bởi @DeleteMapping("{id}"), xử lý các HTTP DELETE request tới "/products/{id}"
   + Phương thức này sử dụng **productsRepository.deleteById(id).join()** để xóa sản phẩm từ repository.
   + Nếu sản phẩm đã được xóa thành công (productDeleted khác null), nó ghi log thông tin về sản phẩm đã bị xóa và trả về ProductDto của sản phẩm đó với mã trạng thái **HttpStatus.OK**.
   + Nếu không tìm thấy sản phẩm để xóa (productDeleted là null), nó trả về thông báo lỗi "Product not found" với mã trạng thái **HttpStatus.NOT_FOUND**.

```java
@DeleteMapping("{id}")
    public ResponseEntity<?> deleteProductById(@PathVariable("id") String id) {
        Product productDeleted = productsRepository.deleteById(id).join();
        if (productDeleted != null) {
            LOG.info("Product deleted - ID: {}", productDeleted.getId());
            return new ResponseEntity<>(new ProductDto(productDeleted), HttpStatus.OK);
        } else {
            return new ResponseEntity<>("Product not found", HttpStatus.NOT_FOUND);
        }
    }
```

![Architect](/images/8/controller/07.png/?featherlight=false&width=60pc)

8. Tạo phương thức **updateProduct**. Phương thức này được đánh dấu bởi **@PutMapping("{id}")**, xử lý các HTTP PUT request tới "/products/{id}".
   + Phương thức này cập nhật thông tin của một sản phẩm dựa trên id được chỉ định. Nó sử dụng **productsRepository.update(...).join()** để cập nhật sản phẩm trong repository
   + Nếu sản phẩm được cập nhật thành công, nó ghi log thông tin về sản phẩm đã được cập nhật và trả về ProductDto của sản phẩm đó với mã trạng thái **HttpStatus.OK**.
   + Nếu không tìm thấy sản phẩm để cập nhật **(productsRepository.update(...) ném ra một CompletionException)**, nó trả về thông báo lỗi "Product not found" với mã trạng thái **HttpStatus.NOT_FOUND**.

```java
@PutMapping("{id}")
    public ResponseEntity<?> updateProduct(@RequestBody ProductDto productDto,
        @PathVariable("id") String id) {
        try {
            Product productUpdated = productsRepository
                    .update(ProductDto.toProduct(productDto), id).join();
            LOG.info("Product updated - ID: {}", productUpdated.getId());
            return new ResponseEntity<>(new ProductDto(productUpdated), HttpStatus.OK);
        } catch (CompletionException e) {
            return new ResponseEntity<>("Product not found", HttpStatus.NOT_FOUND);
        }
    }
```

![Architect](/images/8/controller/08.png/?featherlight=false&width=60pc)
