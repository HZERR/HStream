# HStream 1.7.2 Full
# Не рекомендуется к использованию из-за скорости работы

<b>HStream - обертка над Stream, позволяющая использовать его повторно</b>

<b>Примеры создания экземпляра класса:</b></br>
1. <code>HStream\<String> stream = HStream.empty();</code>
2. <code>HStream\<String> stream = HStream.of("Hello", "world!");</code>
3. <code>HStream\<String> stream = HStream.of(Stream.of("Hello", "world!"));</code>
4. <code>HStream\<String> stream = HStream.of(Arrays.asList("Hello", "world!"));</code>
5. <code>HStream\<String> stream = HStream.of(Collections.emptyEnumeration());</code>

<b>Пример использования:</b></br>
```java
// imports
import ru.hzerr.HStream;

public class CtMethodBytecodeBuilder {

  private CtMethodByteCodeBuilder() {}
  
  private HStream<CtMethod> methods;
  
  public static CtMethodByteCodeBuilder init(CtClass clazz) {
     CtMethodByteCodeBuilder builder = new CtMethodByteCodeBuilder();
     builder.methods = HStream.of(clazz.getDeclaredMethods());
     return builder;
  }
  
  public CtMethodByteCodeBuilder filter(Predicate<CtMethod> predicate) {
     methods.filter(predicate);
     return this;
  }
  
  public CtMethodByteCodeBuilder filterByNames(String... names) {
      HStream<String> namesHStream = HStream.of(names);
      methods.filter(method -> namesHStream.anyMatch(name -> method.getName().equals(name)));
      return this;
  }
  
  public CtMethodByteCodeBuilder removeModifiers(Integer... modifiers) {
      HStream<Integer> modifiersStream = HStream.of(modifiers);
      methods.forEach(method -> modifiersStream.forEach(modifier -> method.setModifiers(method.getModifiers() & ~modifier)));
      return this;
  }
  
  public void setEmptyBody() { 
      methods.forEach(method -> ExceptionWrapper.run(() -> method.setBody("return;"))); 
  }
}
```