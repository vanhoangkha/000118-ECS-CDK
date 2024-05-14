---
title: " Create Controller"
date: "`r Sys.Date()`"
weight: 8
chapter: false
pre: "<b>8.8</b>"
---

#### Create ProductDTO

1. Create a directory named **`dto`** and a new file named **`ProductDto`** within it.

   ![Architect](/images/8/controller/01.png/?featherlight=false&width=60pc)

2. Define the `ProductDTO` as follows:

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


#### Create Controller

3. In the `ProductsController` class, initialize `productsRepository`:
   - Declare **`productsRepository`** as a dependency injection through the constructor, used to access data from products in the database.
   - Initialize **`productsRepository`** in the constructor using **`@Autowired`**, allowing Spring to automatically inject `productsRepository` into the controller.

```java
   private final ProductsRepository productsRepository;

   @Autowired
   public ProductsController(ProductsRepository productsRepository) {
       this.productsRepository = productsRepository;
   }
```
![Architect](/images/8/controller/03.png/?featherlight=false&width=60pc)

4. Create the **getAllProducts** method. This method is annotated with **@GetMapping**, indicating that it handles HTTP GET requests to the specified path (`/products`). The method includes:

   - Logging the information "Get all products".
   - Creating an empty list `productsDto` to store `ProductDto` objects.
   - Using `productsRepository.getAll().items().subscribe(...)` to retrieve a list of products from the repository.
   - For each product retrieved from the repository, it creates a corresponding `ProductDto` and adds it to `productsDto`.
   - Finally, the method returns a `ResponseEntity<List<ProductDto>>` containing the created `productsDto` list with an HTTP status of `HttpStatus.OK`.

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

5. Create the **getProductById** method. This method is annotated with **@GetMapping("{id}")**, indicating that it handles HTTP GET requests to "/products/{id}", where `{id}` is a path variable.

   - This method retrieves the `id` from the path and uses `productsRepository.getById(id).join()` to wait for and retrieve the `Product` from the repository.
   - If the `product` is not `null`, it returns the `ProductDto` of that product with an HTTP status of `HttpStatus.OK`.
   - If the `product` is `null`, it returns an error message "Product not found" with an HTTP status of `HttpStatus.NOT_FOUND`.

   ```java
   @GetMapping("{id}")
   public ResponseEntity<?> getProductById(@PathVariable("id") String id) {
       Product product = productsRepository.getById(id).join();
       if (product != null) {
           return new ResponseEntity<>(new ProductDto(product), HttpStatus.OK);
       } else {
           return new ResponseEntity<>("Product not found", HttpStatus.NOT_FOUND);
       }
   }
```
![Architect](/images/8/controller/05.png/?featherlight=false&width=60pc)

6. Create the **createProduct** method. This method is annotated with **@PostMapping**, indicating that it handles HTTP POST requests to "/products".

   - This method creates a Product from the ProductDto sent in the request (`productDto`), assigns a new **id** using **UUID.randomUUID().toString()**, and then saves **productCreated** to productsRepository using **productsRepository.create(productCreated).join()**.
   - After successfully creating the product, it logs information about the created product and returns the ProductDto of that product with an HTTP status of **HttpStatus.CREATED**.

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

7. Create the **deleteProductById** method. This method is annotated with **@DeleteMapping("{id}")**, indicating that it handles HTTP DELETE requests to "/products/{id}".

   - This method uses **productsRepository.deleteById(id).join()** to delete the product from the repository.
   - If the product is successfully deleted (productDeleted is not null), it logs information about the deleted product and returns the ProductDto of that product with an HTTP status of  **HttpStatus.OK**.
   - If no product is found to delete (productDeleted is null), it returns an error message "Product not found" with an HTTP status of **HttpStatus.NOT_FOUND**..

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

8. Create the **updateProduct** method. This method is annotated with **@PutMapping("{id}")**, indicating that it handles HTTP PUT requests to "/products/{id}".

   - This method updates the information of a product based on the specified id. It uses 88productsRepository.update(...).join()88 to update the product in the repository.
   - If the product is updated successfully, it logs information about the updated product and returns the ProductDto of that product with an HTTP status of 88HttpStatus.OK88.
   - If no product is found to update (**productsRepository.update(...)** throws a **CompletionException**), it returns an error message "Product not found" with an HTTP status of  **HttpStatus.NOT_FOUND**.

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
