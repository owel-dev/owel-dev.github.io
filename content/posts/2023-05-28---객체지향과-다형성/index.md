---
title: "객체지향에서 다형성이란 무엇인가와 사용 이유"
date: "2023-05-28T17:00:00.169Z"
template: "post"
draft: false
slug: "/posts/what-is-polymorphism"
category: "OOP"
tags:
  - "OOP"
  - "Java"
description: "우리를 수정 지옥에서 벗어나게 해줄 다형성에 대해 알아보겠습니다."
socialImage: ""
---

# 객체지향에서 다형성이란 무엇인가와 사용 이유

객체지향 프로그래밍에서 다형성이란  
특정 객체가 여러가지 형태로 변화할 수 있는 성질을 말합니다.

객체지향 프로그래밍의 중요한 목적 중 하나는  
고치기 쉬운(== 변화에 유연한) 코드를 작성할 수 있도록 돕는 것이며  
그 중 Interface를 이용한 다형성은 고치기 쉬운 코드를 작성하기 위한 중요한 개념입니다.

이 글에서는 Java와 Interface를 이용하여 다형성이 코드에서 어떻게 구현되며  
어떻게 고치기 쉬운 코드를 만들어주는지에 대해 설명해 보겠습니다.<br/>

## 수정 지옥의 발생

```java
// Cat.java
-------------------------------------------
public class Cat {
	void makeSound() {
		System.out.println("야옹~");
	}
}

// Main.java
-------------------------------------------
public class Main {
    public static void main(Stirng[] args) {
        Cat cat = new Cat();

        cat.makeSound();
    }
}

// 출력
-------------------------------------------
야옹~


```

위 코드에서 `Main` 클래스는 `Cat` 클래스를 사용하고 있습니다.  
`Cat` 클래스의 인스턴스를 생성하고 변수에 할당한 후  
해당 변수를 이용해 `Cat` 클래스의 `makeSound()` 메서드를 호출하여 고양이 울음소리를 출력하고 있습니다.

그런데 `Main` 클래스에서 고양이 울음소리가 아니라  
강아지 울음소리(”멍멍!”)를 출력해야 하는 변경사항이 생긴다면 어떻게 해야 할까요?

`Cat` 클래스의 `makeSound()` 메서드의 내용을 `System.out.println("멍멍!");` 으로 바꾸자니  
클래스 이름은 `Cat` 인데, 강아지 울음소리를 내는 메서드를 가지고 있다? 뭔가 맞지 않습니다.

그리고, `Main` 클래스가 아닌 다른 클래스에서 `Cat` 클래스가 필요할 수도 있으므로  
`Dog` 라는 이름의 클래스를 새로 만드는 것이 바람직해 보입니다.
</br>

```java
// Cat.java
-------------------------------------------
public class Cat {
	void makeSound() {
        System.out.println("야옹~");
    }
}

// Dog.java
-------------------------------------------
public class Dog {
    void makeHowl() {
        System.out.println("멍멍!");
    }
}

// Main.java
-------------------------------------------
public class Main {
    public static void main(Stirng[] args) {
        Cat cat = new Cat();

        cat.makeSound();
    }
}
```

자, `Dog` 클래스를 새로 만들어 보았습니다.  
이제 `Main` 클래스를 고양이 울음소리가 아닌,  
강아지 울음소리를 출력하는 프로그램으로 만들기 위해서는 3군데의 코드를 고쳐야 합니다.

1. `new Cat();` → `new Dog();`
2. `Cat cat` → `Dog dog;`
3. `cat.makeSound();` → `dog.makeHowl();`

`Main` 클래스는 고작 2줄짜리 프로그램인데 고쳐야 할 부분이 한 두 군데가 아니네요.  
만약, Main 클래스 파일의 코드가 1000줄이었고, 코드 여기저기서

- `Cat` 클래스의 인스턴스를 생성하고
- 인스턴스를 변수에 할당받고
- 해당 변수를 사용하여 `Cat` 클래스의 메서드를 호출하기도 하고
- **`Main` 클래스만이 아닌 다른 많은 클래스에서 `Cat` 클래스를 사용하고 있었고  
  그 중 100개의 클래스의 `Cat` 클래스를 `Dog` 클래스로 바꿔야 하는 상황이라면**

`Cat` 클래스를 `Dog` 클래스로 바꿔야 하는 변경사항이 생길 시  
수많은 곳의 코드를 고쳐야 하는 지옥이 펼쳐질 것입니다..

그런 지옥을 예방하기 위한 방법으로  
객체지향 프로그래밍에서는 **다형성**이란 기능을 제공하며  
Java에서는 이를 Interface 라는 것을 이용하여 구현할 수 있습니다.
<br/>

## Interface를 이용한 다형성 구현

```java
// Animal.java
-------------------------------------------
Interface Animal {
	void makeSound();
}

// Cat.java
-------------------------------------------
public class Cat implements Animal{

    @Overide
    void makeSound() {
        System.out.println("야옹~");
    }
}

// Dog.java
-------------------------------------------
public class Dog implements Animal{

    @Overide
    void makeSound() {
        System.out.println("멍멍!");
    }
}
```

`Animal` 이라는 Interface를 만들어 보았습니다.  
Java에서는 Interface를 구현한(implements) 클래스는  
반드시 Interface의 멤버함수를 Overide(재정의) 하도록 하는 규칙이 있습니다.

```java
// Main.java
-------------------------------------------
public class Main {
    public static void main(Stirng[] args) {
        // Interface 타입의 변수는
        // 자신을 구현한 클래스 타입의 인스턴스를 할당받을 수 있다.
		Animal animal = new Cat();

        animal.makeSound();
    }
}
```

Interface를 만들었으니 이제 이를 이용하여 `Main` 클래스의 코드를 바꿔보겠습니다.

`Cat` 클래스의 인스턴스를 할당하는 변수의 타입을 `Animal` 로 바꾸었고  
그 아랫줄에서 `animal.makeSound()` 를 호출하도록 하였습니다.

Interface를 이용하여 다형성을 사용하게 되면

- Interface 타입의 변수는 구현 클래스의 타입이라면 어떤 타입의 인스턴스도 할당 받을 수 있고
- Interface 를 구현한 클래스들은 같은 이름의 메서드를 가지고 있다는 것이 보장되기에

`Main` 클래스의 코드를 `Animal` Interface를 이용하여 위와 같이 고치면  
`Main` 클래스를 고양이 울음소리를 내는 프로그램에서  
강아지 울음소리를 내는 프로그램으로 바꿔야 하는 변경사항이 발생했을 때

`new Cat()` -> `new Dog()` 부분의 코드만 바꿔주면  
나머지 부분의 코드는 바꾸지 않아도 프로그램은 정상적으로 동작할 것입니다.

Interface를 이용하지 않은 경우에는 3군데를 고쳐야 했었는데 이제는 1군데라니..  
`Main` 클래스는 변경사항이 발생했을 때 이전보다 훨씬 유지보수하기 편한 코드가 되었습니다.  
이렇듯 Interface를 이용하여 다형성을 사용하면, 변경사항 발생 시 더욱 고치기 쉬운 코드를 작성할 수 있습니다.
<br/>

## 팩토리 패턴을 사용해 더욱 고치기 쉬운 코드 만들기

그런데 아직 조금 더 개선할 부분이 남아있네요.  
Interface를 이용하여 `Main` 클래스를 이전보다 더욱 고치기 쉬운 코드로 만들었지만  
`new Cat()` -> `new Dog()`로 바꾸는 부분마저 고치지 않아도 되도록 할 수는 없을까요?

고칠 곳이 3군데에서 1군데로 줄기는 했지만  
만약 수 많은 클래스에서 `Cat` 클래스를 사용하고 있으며  
그 중 100개의 파일에서 `new Cat()` 을 `new Dog()` 로 고쳐줘야 한다면  
이것도 여간 귀찮은 일이 아닐 수 없습니다.

그래서 아래와 같은 방법을 이용하여 더욱 유지보수하기 편한 코드로 개선할 수 있습니다. </br>

```java
// AnimalMaker.java
-------------------------------------------
public class AnimalMaker {
    public Cat makeAnimal() {
        return new Cat();
    }
}

// Main.java
-------------------------------------------
public class Main {
    public static void main(Stirng[] args) {
        AnimalMaker animalMaker = new AnimalMaker();

        Animal animal = animalMaker.makeAnimal();

        animal.makeSound();
    }
}

// Main2.java
-------------------------------------------
public class Main2 {
    public static void main(Stirng[] args) {
        AnimalMaker animalMaker2 = new AnimalMaker();

        Animal animal2 = animalMaker2.makeAnimal();

        animal2.makeSound();
    }
}

// Main3.java
-------------------------------------------
public class Main3 {
    public static void main(Stirng[] args) {
        AnimalMaker animalMaker3 = new AnimalMaker();

        Animal animal3 = animalMaker3.makeAnimal();

        animal3.makeSound();
    }
}
```

`Cat` 클래스의 인스턴스를 생성한 후 이를 반환하는 `makeAnimal()` 라는 메서드를 가진  
`AnimalMaker` 클래스를 만들었습니다.

`Main`, `Main2`, `Main3` 클래스에서는  
`AnimalMaker` 클래스를 이용하여 `Cat` 클래스를 간접적으로 사용하고 있습니다.

이제 `Cat` 클래스를 `Dog` 클래스로 바꾸어야 하는 변경사항이 발생하면

`Main`, `Main2`, `Main3` 각각의 클래스에서  
`new Cat()` -> `new Dog()`로 수정할 필요 없이  
`AnimalMaker` 클래스에서만 `new Cat()` -> `new Dog()`로 수정하면 되도록  
더욱 고치기 쉬운 코드를 만들어 보았습니다.
<br/>
`AnimalMaker` 처럼 특정 객체를 생성하고 이를 반환하는 메서드를 가진 클래스를 사용하여  
객체의 생성과 사용을 직접 하는 것이 아닌 간접적으로 하는 방식을 **팩토리 패턴** 이라고 합니다.

객체지향 패러다임에서 다형성과 팩토리 패턴 같은 객체지향의 특성들을 사용하면  
변경사항이 발생했을 때 고치기 쉬운, 유지보수하기 좋은 유연한 코드를 작성할 수 있습니다.
