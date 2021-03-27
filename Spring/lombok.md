## @Getter and @Setter
앞으로는 `public int getFoo() {return foo;}` 사용할 필요 없어!! 😉

`field`에 `@Getter` 와/나 `@Setter` annotation을 사용하면 lombok으로 default getter/setter를 자동으로 생성할 수 있다.

default getter는 단순히 해당 field를 return 한다. 이때, 만약 filed의 이름이 `foo`라면 `getFoo` 라는 이름이 붙는다. (field의 타입이 boolean이면 `isFoo`)
default setter는 `void`를 return 한다. 만약 field의 이름이 `foo`라면 `setFoo`라는 이름이 붙는다. 그리고 field와 동일한 타입의 parameter 1개를 사용한다.

`AccessLevel`을 특별히 지정한게 아니라면, 생성된 getter/setter 메소드는 기본적으로 `public` 이 된다. (이는 아래 예시에서 보여줄 예정이다.)
참고로 Legal Access level에는 `PUBLIC`, `PROTECTED`, `PACKAGE`, `PRIVATE`가 있다.

class에도 `@Getter` 와/나 `Setter` annotation을 사용할 수 있다. 이러면 해당 클래스의 모든 non-static field에 annotation을 사용하는 것과 같다.

특별하게 `AccessLevel.None` access level을 사용해서 모든 field에 대해 getter/setter 생성을 수동으로 비활성화 할 수 있다.
이렇게 하면 클래스에 대한 `@Getter`, `@Setter`, `@Data` annotation의 동작을 override (재정의) 할 수 있다.

이미 생성된 메소드에 대해 annotation을 하고 싶으면, `onMethod=@__({@AnnotationsHere})`; 을 사용한다.
이미 생성된 메소드의 한 parameter에대해 annotation을 하고 싶으면, `onParam=@__({@AnnotationsHere})` 을 사용한다.
근데 이거 아직 experimental feature니까 조심해야함. 더 자세히 알고 싶으면 onX feature 문서 찾아보셈!

NEW in lombok v1.12.0:

이제 field에 있는 javadoc가 생성된 getter/setter에 복사된다!
일반적으로 모든 텍스트가 복사되고, `@return` 은 getter 로 이동하고, `@param` line들은 setter로 이동한다.
(이동했다는건, field의 javadoc에서는 제거됐다는 뜻이다.)

또한 각 getter/setter에 대해 unique text를 define 할 수 있다! 그러려면 `GETTER` 와/나 `SETTER` 라는 이름의 `section'을 생성해햐 한다.
section은 2개 이상의 dash를 포함하는 javadoc의 line으로, 'GETTER' 또는 'SETTER' 텍스트 다음에 2개 이상의 dash를 포함하고 line에 다른 건 포함하지 않는다.

section을 사용하면, 해당 section에 대한 `@return` 과 `@param` stripping은 수행되지 않고 무시된다. (@return 또는 @param line 이 section으로 이동한다.)

### with Lombok
```java
import lombok.AccessLevel;
import lombok.Getter;
import lombok.Setter;

public class GetterSetterExample {
  /**
   * 이 사람의 나이 💙
   * 
   * @param age | 이 사람의 나이의 New value 🧡
   * @return | 이 사람의 나이의 current value 💚
   */
  @Getter @Setter private int age = 10;
  
  /**
   * 이 사람의 이름
   * -- SETTER --
   * 이 사람의 이름 변경
   * 
   * @param name | new value.
   */
  @Setter(AccessLevel.PROTECTED) private String name;
  
  @Override public String toString() {
    return String.format("%s (age: %d)", name, age);
  }
}
```

### Vanilla Java
```java
public class GetterSetterExample {
  /**
   * 이 사람의 나이 💙
   */
  private int age = 10;

  /**
   * 이 사람의 이름
   */
  private String name;
  
  @Override public String toString() {
    return String.format("%s (age: %d)", name, age);
  }
  
  /**
   * 이 사람의 나이 💙
   *
   * @return | 이 사람의 나이의 current value 💚
   */
  public int getAge() {
    return age;
  }
  
  /**
   * 이 사람의 나이 💙
   *
   * @param age | 이 사람의 나이의 New value 🧡
   */
  public void setAge(int age) {
    this.age = age;
  }
  
  /**
   * 이 사람의 이름 변경
   *
   * @param name | new value.
   */
  protected void setName(String name) {
    this.name = name;
  }
}
```
