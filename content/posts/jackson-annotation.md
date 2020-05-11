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

## 常用注解

### 同时影响序列化/反序列化的注解

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
    "id":1，
    "name":"My bean"
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
        "id":1，
        "name":"My bean"
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
    "bean id":1，
    "name":"My bean"
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
    "id":1，
    "name":"My bean",
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

### @JsonView

通过代码控制可见性：

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

可控制包括：fieldVisibility，getterVisibility，isGetterVisibility，setterVisibility，creatorVisibility
可见性包括：ANY，DEFAULT，NON_PRIVATE，NONE，PROTECTED_AND_PRIVATE 和 PUBLIC_ONLY。

#### @JsonIgnoreProperties

忽略掉所有指定的字段：

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

#### @JsonIgnore

和 `@JsonIgnoreProperties` 类似，可忽略单个字段，上面的等价于：

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

### 序列化注解 Java -> Json

#### @JsonInclude

控制当字段值为 null/空/默认值 时是否排除：

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

#### @JsonGetter

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
"id":1，
"name":"MB BEAN"

}
```

#### @JsonAnyGetter

#### @JsonRawValue

指明该字段的值直接输出（就是序列化后的值），不做序列化：

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

放上`@JsonRawValue`:

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

#### @JsonSerialize

指明特定的序列化：

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

#### @JsonValue

完全自定义序列化输出：

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

### 反序列化注解 Json -> Java

#### @JsonSetter

#### @JsonAnySetter

#### @JacksonInject

#### @JsonDeserialize

#### @JsonCreator

#### @JsonValue

## 自定义注解

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

## Mixin 注解

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

## 禁用注解

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

---

<i class="fas fa-map-marker-alt"></i>&nbsp;&nbsp; 如果上面的注解不能满足，可以定制 Jackson 的 `Serializer` & `Deserializer` 来达到精选控制序列化/反序列化的目的。
