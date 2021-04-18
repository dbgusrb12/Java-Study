Inheritance
===

# 상속(Inheritance) 이란?

Oracle 공식 가이드에 따르면,   
**Java 에서 클래스는 다른 클래스에서 파생될 수 있으므로 해당 클래스에서 필드와 메서드를 상속한다.**   
라고 나와있다.   

상속(Inheritance) 이란 기존에 존재하던 클래스를 기반으로 새로운 클래스를 구현하는 것이다.

# 상위 클래스 (superclass)

상속을 할 때 기반이 되는 클래스이며,   
기본 클래스 (basic class), 부모 클래스 (parent class) 라고도 한다.

# 하위 클래스 (subclass)

상속을 할 때 파생된 클래스를 말하며,   
파생 클래스 (derived class), 확장 클래스 (extended class), 자식 클래스 (child class) 라고도 한다.

# 상속의 특징

- 한번에 여러 클래스를 상속 받을 수 없다. (single inheritance)
- 부모 클래스가 명시적으로 작성되어 있지 않은 모든 클래스는 암묵적으로 `Object` 클래스를 상속받는다.   
  (모든 클래스는 `Object` 클래스의 서브 클래스이다.)
- 상속 받은 클래스를 상속 받아 클래스를 구현 할 수 있다. (Multi-level inheritance)






> 웹문서
> - [The Java Tutorials(Inheritance)](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)