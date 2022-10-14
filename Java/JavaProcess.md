# Java 동작 과정

## 기본 순서
1. .java 파일을 JDK의 javac 도구를 통해 바이트코드로 변환한다.
2. 바이트코드를 java로 실행시킨다. Classloader로 읽어오고, Bytecode verifier로 바이트코드를 검증하며, machine code generator로 바이트코드를 머신코드로 변환하여 JVM에 전달한다.
3. 위에서부터 읽으며 클래스를 만나면, 
   1. 클래스 로드한적이 있나 확인 후 없다면 rt.jar에서 클래스를 가져오고, 없다면 워킹디렉토리에서 가져온다.
   2. static member를 메모리의 static 영역에 초기화 한다.
   3. 상속관계 파악을 하여 클래스 로드 과정으로 돌아간다.
   4. 클래스 로드가 완료되면 마지막으로 main을 수행한다.
```java
public class SimpleUrlController{
	public static void main(String[] args) {
		System.out.println();
	}
}
```
이러한 코드가 있다면 SimpleUrlController 클래스를 로드한적이 있나 확인하고, 없다면 로드한다. 그 후 static member를 초기화 하는데 여기에는 없다. 다음으로 상속관계 파악을 하는데, 아무것도 없다면 Object를 상속하므로, 다시 오브젝트 클래스를
로드한적이 있는지 체크하는것으로 돌아간다. static member초기화와 상속관계 파악이 완료 되었다면, 메인을 수행한다.

System 클래스를 만나면, 동일하게 클래스로드 - static member 초기화 - 상속관계 파악등의 과정을 거치게되는데, 
여기서 PrintWriter타입의 out이라는 이름의 static 변수를 예로 들어보면, 먼저 static member로써 메모리 static 영역에 4바이트가 할당되고(레퍼런스 타입이므로 주소값을 저장하려고), null로써 디폴트 초기화가 된다.  
out변수는 new PrintWriter()로 객체를 변수에 할당하는데, 여기서 new의 효과는, 메모리의 에덴영역에 공간을 할당하는것이고, PrintWriter()는 생성자 효과를 나타낸다.
- 생성자는 non-static member(필드 + 메소드)를 초기화 하는 기능을 한다. 필드는 객체 안에 디폴트 초기화, 메소드는 메모리의 메소드 영역에 저장되며 객체와 링크된다.
- 생성자는 가장 위에 super()를 호출한다. 즉, 부모 클래스의 생성자를 호출하는데, 자식 클래스의 필드가 디폴트 초기화 된 다음 부모 클래스의 non-static member (디폴트)초기화가 일어난다.
- 최상단 Object 클래스의 생성자까지 호출이 되면 더이상 올라가지 않고 Object 클래스의 생성자가 수행되며, 다시 자식 객체로 내려오면서 생성자에서 수행하는 명시적 초기화가 상속관계 위에서부터 쭉 발생하게된다.
- 모든 과정이 완료되었다면 주소값이 부여되며, static 영역에 null로써 초기화 되었던 out 변수에 주소값이 입력되며 레퍼런스가 생성된다.