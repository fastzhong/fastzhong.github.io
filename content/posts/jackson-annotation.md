+++
title = "Jackson 常用注解"
date = 2020-05-11T13:08:47+08:00
readingTime = true
categories = ["编程"]
tags = ["java"]
toc = true
+++

Json 满天飞，Jackson 作为 Json 序列化/反序列化的最强大工具库，可以说是项目中必备的，有必要记录一下。

<!--more-->

### 通用注解（序列化/反序列化）

#### @JsonRootName

无 `@JsonRootName` 时：

```java
public class MyBean {
    public int id = 1;
    public String name = "My Bean";
}
```

对应的 Json：

```json
{
    "id": 1，
    "name": "My bean"
}
```

有 `@JsonRootName` 时：

```java
@JsonRootName(value = "bean")
public class MyBean {
    public int id = 1;
    public String name = "My Bean";
}
```

对应的 Json：

```json
{
    "bean": {
        "id": 1，
        "name": "My bean"
    }
}
```

#### @JsonProperty

自定义对应的 Json 字段名：

```java
public class MyBean {
    @JsonProperty("bean id")
    public int id = 1;
    public String name = "My Bean";
}
```

对应的 Json：

```json
{
    "bean id": 1，
    "name": "My bean"
}
```

#### @JsonFormat

自定义对应的 Json 值格式：

```java
public class MyBean {
    public int id = 1;
    public String name = "My Bean";
    @JsonFormat(
      shape = JsonFormat.Shape.STRING,
      pattern = "dd-MM-yyyy hh:mm:ss")
    public Date eventDate = new Date();
}
```

对应的 Json：

```json
{
    "id": 1，
    "name": "My bean",
    "eventDate": "11-05-2020 22:30:00"
}
```

#### @JsonUnwrapped

```java
public class MyBean {
    public int id = 1;

    @JsonUnwrapped
    public Name name = new Name();

    public static class Name {
        public String firstName = "Mr";
        public String lastName = "Bean";
    }
}
```

name 字段将会被展开值取代，对应的 json：

```json
{
    "id": 1,
    "firstName": "John",
    "lastName": "Doe"
}
```

#### @JsonView

通过定义 View，代码层控制可序列化/反序列化的字段：

```java
public class Views {
    public static class Public {}
    public static class Internal extends Public {}
}

public class MyBean {
    @JsonView(Views.Public.class)
    public int id = 1;

    @JsonView(Views.Public.class)
    public String name = "My Bean";

    @JsonView(Views.Internal.class)
    public String password = "iloveu";
}
```

下面测试一下：

```java
@Test
public void whenSerializingUsingJsonView()
  throws JsonProcessingException {
    MyBean bean = new MyBean();

    String result = new ObjectMapper()
      .writerWithView(Views.Public.class)
      .writeValueAsString(bean);

    assertThat(result, containsString("1"));
    assertThat(result, containsString("My Bean"));
    assertThat(result, not(containsString("iloveu")));
}
```

#### @JsonFilter

类似 `@JsonView`，通过定义 Filter，代码层控制可序列化/反序列化的字段：

```java
@JsonFilter("myFilter")
public class MyBean {
    public int id = 1;
    public String name = "My Bean";
    public Token token = "iloveu";
}
```

```java
MyBean bean = new MyBean();

FilterProvider filters
    = new SimpleFilterProvider().addFilter(
        "myFilter",
        SimpleBeanPropertyFilter.filterOutAllExcept("token");

String result = new ObjectMapper()
  .writer(filters)
  .writeValueAsString(bean);

```

#### @JsonIdentityInfo，@JsonManagedReference，@JsonBackReference

如果两个对象互相引用，序列化/反序列化时，`@JsonIdentityInfo` 可以避免陷入无限递归调用：

```java
@JsonIdentityInfo(
  generator = ObjectIdGenerators.PropertyGenerator.class,
  property = "id")
public class ItemWithIdentity {
    public int id;
    public String itemName;
    public UserWithIdentity owner;
}

@JsonIdentityInfo(
  generator = ObjectIdGenerators.PropertyGenerator.class,
  property = "id")
public class UserWithIdentity {
    public int id;
    public String name;
    public List<ItemWithIdentity> userItems;
}
```

UserWithIdentity 和 ItemWithIdentity 相互引用：

```java
@Test
public void whenSerializingUsingJsonIdentityInfo_thenCorrect()
  throws JsonProcessingException {
    UserWithIdentity user = new UserWithIdentity(1, "John");
    ItemWithIdentity item = new ItemWithIdentity(2, "book", user);
    user.addItem(item);

    String result = new ObjectMapper().writeValueAsString(item);

    assertThat(result, containsString("book"));
    assertThat(result, containsString("John"));
    assertThat(result, containsString("userItems"));
}
```

相应的 item：

```json
{
    "id": 2,
    "itemName": "book",
    "owner": {
        "id": 1,
        "name": "John",
        "userItems": [2]
    }
}
```

上面的例子还可以有另外一种方式化解，不通过 id 而是指明父子引用关系：

```java
public class ItemWithIdentity {
    public int id;
    public String itemName;
    @JsonManagedReference
    public UserWithIdentity owner;
}

public class UserWithIdentity {
    public int id;
    public String name;
    @JsonBackReference
    public List<ItemWithIdentity> userItems;
}
```

### 序列化注解 Java -> Json

#### @JsonPropertyOrder

这个控制输出字段的顺序：

```java
@JsonPropertyOrder({ "name", "id" })
public class MyBean {
    public int id;
    public String name;
}
```

序列化输出结果（name 跳到 id 前面）:

```json
{
    "name": "My bean",
    "id": 1
}
```

`@JsonPropertyOrder(alphabetic=true)` 可以按字段名排序输出。

#### @JsonGetter，@JsonAnyGetter

这个取代 `@JsonProperty`，指定该字段的 getter 方法：

```java
public class MyBean {
    public int id = 1;
    public String name = "My Bean";

    @JsonGetter("name")
    public String getMyName() {
        return name.toUpperCase();
    }
}
```

输出：

```json
{
    "id": 1，
    "name": "MB BEAN"
}
```

`@JsonAnyGetter` 则可以对“动态字段”，例如 Map 的 key 进行序列化：

```java
public class MyBean {
    public int id = 1;
    public String name = "My Bean";
    private Map<String, String> properties;

    @JsonAnyGetter
    public Map<String, String> getProperties() {
        return properties;
    }
}
```

properties 里的 attr1、attr2 会被“动态”输出：

```json
{
    "id": 1，
    "name": "My Bean",
    "attr1": "val1",
    "attr2": "val2",
}
```

#### @JsonSerializer

`@JsonSerializer` 指明特定的序列化：

```java
public class MyBean {
    public int id = 1;
    public String name = "My Bean";
    @JsonSerialize(using = OptimizedBooleanSerializer.class)
    public boolean enabled = false;
}
```

给 enabled 指定序列化 OptimizedBooleanSerializer，将序列化 true 的 Json 值为 1 和 false 的 Json 值为 0：

```java
public class OptimizedBooleanSerializer extends JsonSerializer<Boolean> {

    @Override
    public void serialize(Boolean aBoolean, JsonGenerator jsonGenerator,
        SerializerProvider serializerProvider)
    throws IOException, JsonProcessingException {

        if(aBoolean){
            jsonGenerator.writeNumber(1);
        } else {
            jsonGenerator.writeNumber(0);
        }
    }
}
```

通常用来控制对特殊格式的输出。

#### @JsonValue，@JsonRawValue

`@JsonValue` 定义序列化输出：

```java
public class MyBean {
    public int id = 1;
    public String name = "My Bean";

    @JsonValue
    public String toJson() {
        return this.id + ", " + this.name;
    }
}
```

对应的 Json：

```text
"1, My Bean"
```

`@JsonRawValue` 指明该字段的值直接输出（就是序列化后的值），不做序列化：

```java
public class MyBean {
    public int id = 1;
    public String name = "My Bean";
    public String errJson = "{ \"code\" : \"101\", \"msg\": \"输入值无效\"}";
}
```

对应的 Json：

```json
{
    "id":1，
    "name":"My Bean",
    "errJson": "{\"code\" : \"101\", \"msg\": \"输入值无效\"}
}
```

放上 `@JsonRawValue`:

```java
public class MyBean {
    public int id = 1;
    public String name = "My Bean";
    @JsonRawValue
    public String errJson = "{ \"code\" : \"101\", \"msg\": \"输入值无效\"}";
}
```

对应的 Json：

```json
{
    "id":1，
    "name":"My Bean",
    "errJson": {"code" : "101", "msg": "输入值无效"}
}
```

### 反序列化注解 Json -> Java

#### @JsonSetter，@JsonAnySetter

和 `JsonGetter` 类似，`@JsonSetter` 取代 `@JsonProperty`，指定该字段的 setter 方法：

```java
public class MyBean {
    public int id = 1;
    private String name;

    @JsonSetter("name")
    public void setTheName(String name) {
        this.name = name.toLowerCase();
    }
}
```

```java
@Test
public void whenDeserializingUsingJsonSetter()
  throws IOException {

    String json = "{\"id\":1,\"name\":\"MY BEAN\"}";

    MyBean bean = new ObjectMapper()
      .readerFor(MyBean.class)
      .readValue(json);
    assertEquals("my bean", bean.getTheName());
}
```

和 `@JsonAnyGetter` 类似，在反序列化时，`@JsonAnySetter` 对于所有没有匹配的字段进行处理：

```json
{
    "id": 1，
    "name": "My Bean",
    "attr1": "val1",
    "attr2": "val2",
}
```

```java
public class MyBean {
    public int id;
    public String name;
    private Map<String, String> properties;

    @JsonAnySetter
    public void add(String key, String value) {
        properties.put(key, value);
    }
}
```

json 里的 attr1、attr2 会被“动态”注入到 properties 里。

#### @JacksonInject

通过代码赋值，而非从 Json 反序列化中得到：

```java
public class MyBean {
    public int id;
    public String name;

    @JsonInject
    publicc String createdby;
}

String json = "{\"name\":\"My bean\"}";

InjectableValues inject = new InjectableValues.Std()
  .addValue(int.class, 1);
BeanWithInject bean = new ObjectMapper().reader(inject)
  .forType(BeanWithInject.class)
  .readValue(json);

String json = "{\"id\":1, \"name\":\"My Bean\"}";

InjectableValues inject = new InjectableValues.Std().addValue(String.class, "张三");
MyBean bean = new ObjectMapper()
    .reader(inject)
    .forType(MyBean.class)
    .readValue(json);
```

#### @JsonCreator

反序列化时，需要创建 Java 对象，结合 `@JsonProperty`，`@JsonCreator` 用来指明如何创建：

```json
{
    "id": 1,
    "theName": "My Bean"
}
```

```java
public class MyBean {
    public int id;
    public String name;

    @JsonCreator
    public MyBean(
      @JsonProperty("id") int id,
      @JsonProperty("theName") String name) {
        this.id = id;
        this.name = name;
    }
}
```

#### @JsonDeserializer

和 `@JsonSerializer` 类似，`@JsonDesearilizer` 指明特定的反序列化：

```java
public class MyBean {
    public int id = 1;
    public String name = "My Bean";
    @JsonDeserialize(using = OptimizedBooleanDeserializer.class)
    public boolean enabled = false;
}
```

给 enabled 指定反序列化 OptimizedBooleanDeserializer：

```java
public class OptimizedBooleanDeserializer extends JsonDeserializer<Boolean> {

    @Override
    public Boolean deserialize(JsonParser parser, DeserializationContext context) throws IOException {
        String boolStr = parser.getText();
        if ("1".equalsIgnoreCase(boolStr) ||
            "y".equalsIgnoreCase(boolStr) ||
            "yes".equalsIgnoreCase(boolStr) ||
            "t".equalsIgnoreCase(boolStr) ||
            "true".equalsIgnoreCase(boolStr) ||
            "on".equalsIgnoreCase(boolStr))
            return true;
        return false;
    }
}
```

### 控制注解

#### @JsonAutoDetect

通过 Java 的可见性控制序列化/反序列化，如果可见性不匹配则排除：

```java
@JsonAutoDetect(fieldVisibility = JsonAutoDetect.Visibility.PUBLIC)
public class MyBean {
    private int id;
    public String name;
}
```

对应的 Json：

```json
{
    "name": "My bean"
}
```

id 字段被排除。

可见性包括：fieldVisibility，getterVisibility，isGetterVisibility，setterVisibility，creatorVisibility

对应的值包括：ANY，DEFAULT，NON_PRIVATE，NONE，PROTECTED_AND_PRIVATE 和 PUBLIC_ONLY。

#### @JsonIgnoreProperties，@JsonIgnore

忽略掉 `@JsonIgnoreProperties，` 所有指定的字段：

```java
@JsonIgnoreProperties（{“firstName”，“lastName”}）
public class MyBean {
    public int id = 1;
    public String name = "My Bean";
    public String createdby = "张三";
    public String updatedby = "李四";
}
```

对应的 Json：

```json
{
"bean id":1，
"name":"My bean"
}
```

`@JsonIgnore` 和 `@JsonIgnoreProperties` 类似，可忽略单个字段，上面的等价于：

```java
public class MyBean {
    public int id = 1;
    public String name = "My Bean";

    @JsonIgnore
    public String createdby = "张三";

    @JsonIgnore
    public String updatedby = "李四";
}
```

`@JsonIgnore` 出现字段、getter、setter、creator 上任何一个，序列化/反序列化都会忽略，如果 getter 上 `@JsonProperty`，setter 上 `@JsonIgnore`，则该字段“只读” - 只能序列化，不能反序列化。

#### @JsonInclude

序列化时控制当字段值为 null/空/默认值 时是否排除：

```java
@JsonInclude(Include.NON_NULL)
public class MyBean {
    public int id = 1;
    public String name;
}
```

```java
public void whenSerializingUsingJsonInclude()
  throws JsonProcessingException {

    MyBean bean = new MyBean();

    String result = new ObjectMapper()
      .writeValueAsString(bean);

    assertThat(result, containsString("1"));
    assertThat(result, not(containsString("name")));
}
```

### 多态处理

三个注解组合处理多态：

-   @JsonTypeInfo：定义指明具体类型值的属性
-   @JsonSubTypes：通过类型值指明具体的 class
-   @JsonTypeName：定义 class 对应的具体类型值

例子：

```java
public class Zoo {
    public Animal animal;

    @JsonTypeInfo(
      use = JsonTypeInfo.Id.NAME,
      include = As.PROPERTY,
      property = "type")
    @JsonSubTypes({
        @JsonSubTypes.Type(value = Dog.class, name = "dog"),
        @JsonSubTypes.Type(value = Cat.class, name = "cat")
    })
    public static class Animal {
        public String name;
    }

    @JsonTypeName("dog")
    public static class Dog extends Animal {
        public double barkVolume;
    }

    @JsonTypeName("cat")
    public static class Cat extends Animal {
        boolean likesCream;
        public int lives;
    }
}
```

当我们序列化时：

```java
@Test
public void whenSerializingPolymorphic()
  throws JsonProcessingException {
    Zoo.Dog dog = new Zoo.Dog("lacy");
    Zoo zoo = new Zoo(dog);

    String result = new ObjectMapper()
      .writeValueAsString(zoo);

    assertThat(result, containsString("type"));
    assertThat(result, containsString("dog"));
}
```

得到：

```json
{
    "animal": {
        "type": "dog",
        "name": "lacy",
        "barkVolume": 0
    }
}
```

下面时反序列化的例子：

```java
@Test
public void whenDeserializingPolymorphic_thenCorrect()
throws IOException {
    String json = "{\"animal\":{\"name\":\"lacy\",\"type\":\"cat\"}}";

    Zoo zoo = new ObjectMapper()
      .readerFor(Zoo.class)
      .readValue(json);

    assertEquals("lacy", zoo.animal.name);
    assertEquals(Zoo.Cat.class, zoo.animal.getClass());
}
```

### 自定义注解

```java
@Retention(RetentionPolicy.RUNTIME)
    @JacksonAnnotationsInside
    @JsonInclude(Include.NON_NULL)
    @JsonPropertyOrder({ "name", "id", "value" })
    public @interface CustomAnnotation {}

@CustomAnnotation
public class MyBean {
    public int id = 1;
    public String name = "My Bean";
    public String value;
}
```

测试一下：

```java
@Test
public void whenSerializingUsingCustomAnnotation()
  throws JsonProcessingException {
    MyBean bean = new MyBean();
    String result = new ObjectMapper().writeValueAsString(bean);

    assertThat(result, containsString("My bean"));
    assertThat(result, containsString("1"));
    assertThat(result, not(containsString("value")));
}
```

### Mixin 注解

此类注解不直接作用在字段上：

```java
public class MyBean {
    public int id = 1;
    public String name = "My Bean";
    public Token token = new Token();
}

@JsonIgnoreType
public class Token {
    public String key = "ilove";
    public int effectiveYears = 10000;
}
```

属于 Token 类的字段都被排除，下面测试一下：

```java
@Test
@Test
public void whenSerializingUsingMixInAnnotationt()
  throws JsonProcessingException {
    MyBean bean = new MyBean();
 
    String result = new ObjectMapper().writeValueAsString(bean);
    assertThat(result, containsString("token"));
 
    ObjectMapper mapper = new ObjectMapper();
    mapper.addMixIn(MyBean.class, Token.class);
 
    result = mapper.writeValueAsString(item);
    assertThat(result, not(containsString("token")));
}
```

### 禁用注解

```java
public class MyBean {
    @JsonProperty("bean id")
    public int id = 1;

    @JsonIgnore
    public String name = "My Bean";
}
```

下面测试一下：

```java
@Test
public void whenDisablingAllAnnotations()
  throws IOException {
    MyBean bean = new MyBean();

    ObjectMapper mapper = new ObjectMapper();
    mapper.disable(MapperFeature.USE_ANNOTATIONS);
    String result = mapper.writeValueAsString(bean);

    assertThat(result, not(containsString("bean id")));
    assertThat(result, containsString("name"));
}
```

可以看到注解起不到效果。

---

<i class="fas fa-map-marker-alt"></i>&nbsp;&nbsp; 如果上面的注解不能满足，任何时候可以在 class 和字段级别 定制 `Serializer` & `Deserializer` 来达到精细控制序列化/反序列化的目的。
