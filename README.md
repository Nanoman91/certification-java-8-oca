# Certification Oracle Java 8 - Niveau OCA

Ce document a pour but de regrouper les notes et les conseils concernant la certification Oracle Java 8 niveau OCA.  
Ces différents éléments sont tirés du livre officiel **OCA: Oracle Certified Associate Java SE 8 Programmer I Study Guide**.

**Le document est en cours d'élaboration, les chapitres 4, 5 et 6 ne sont pas encore terminés.**

## Sommaire

1. [Conseils pour l'examen](#conseils-pour-lexamen)
2. [Chapitre 1 - Java building blocks](#chapter-1-java-building-blocks)
3. [Chapitre 2 - Operators and Statements](chapter-2-operators-and-statements)
4. [Chapitre 3 - Core Java APIs](chapter-3-core-java-apis)
5. [Chapitre 4 - Methods and Encapsulation](chapter-4-methods-and-encapsulation)
6. [Chapitre 5 - Class Design](chapter-5-class-design)
7. [Chapitre 6 - Exceptions](chapter-6-exceptions)

## Conseils pour l’examen

- Bien prendre le temps de lire chaque question :
	- attention au vocabulaire employé, par exemple la déclaration des variables avec le mot local ou de classe
- À partir du moment où il y a une question avec une réponse qui indique un problème de compilation, il faut bien être attentif à ces différents éléments :
	- la syntaxe : 
		- est-ce que tous les points virgules sont présents ?
		- est-ce que les blocs de code sont bien ouverts et fermés; attention aux parenthèses et aux accolades
		- est-ce que les éléments sont correctement écrits ?
		- vérifier le typage et la déclaration des éléments
	- le scope :
		- il faut bien vérifier la déclaration des différentes variables et dans quel scope elles sont déclarées
	- les imports :
		- si le code ne commence pas à la ligne 1 on considère les imports comme valident
		- si il commence à la ligne 1, il faut vérifier que tout est correct
- Attention aux éléments de code écrits sur une seule ligne
- Attention aux structures de code qui ne nécessitent pas forcément l'ouverture et la fermeture d'accolades. Souvent l'instruction suivante celle qui est dans le bloc est indenté pour faire croire qu'elle est dans même le bloc de code alors qu'elle ne l'est pas :
	
	```java
	while (x < 10)
		y--;
		x++;
	```	
	Seule l'instruction `y--` est dans le block `while`
	
**[Back to top](#sommaire)**

## Chapter 1 JAVA Building Blocks

- **WARNING :**

	```java
	/*
 	* /* ferret */ 
	*/
	``` 
	That code does not compile because the first `*/` is interpreted as the end of the bloc comment and the second `*/` does not have a start.
- The scope of a class is not essential. If nothing is precised, it is `public`.
- To compile a class `javac Zoo.java ` (extension `.java` needed) and to execute it `java Zoo` (`main()` method needed). 
- Rules of java file :
	- Each file can contain only one public class.
	- The filename must match the class name, including case, and have a `.java` extension.
- using `L` instead of `l` for long declaration is appreciated
- **WARNING :** be careful with numbers declaration especially these one 
	
	```java
	System.out.println(56); // 56 normal int
	System.out.println(0b11); // 3 binary declaration
	System.out.println(017); // 15 octal declaration (starts with 0)
	System.out.println(0x1F); // 31 hexadecimal declaration
	int million2 = 1_000_000; // valid declaration for 1 million 
	```
- **WARNING**	

	```java
	int i1, i2, i3 = 0; // only i3 is initialized to 0
	```
- variable declaration (page 28) :
	- The name must begin with a letter or the symbol $ or _. 
	- Subsequent characters may also be numbers.
	- Do not use Java reserved words
- **When you see a nonstandard identifier, be sure to check if it is legal. If not, you get to mark the answer “does not compile” and skip analyzing everything else in the question.**
- Reminder on scope of variables 
	- Local variables—in scope from declaration to end of block
	- Instance variables—in scope from declaration until object garbage collected
	- Class variables—in scope from declaration until program ends
- Elements of a class **PIC** : **P**ackage **I**mport **C**lass

|       Element       |       Example       | Required |       Where does it go?       |
|:-------------------:|:-------------------:|:--------:|:-----------------------------:|
| Package declaration |     package abc;    |    No    |     First line in the file    |
|  Import statements  | import java.util.*; |    No    | Immediately after the package |
|  Class declaration  |    public class C   |    Yes   |  Immediately after the import |
|  Field declarations |      int value;     |    No    |    Anywhere inside a class    |
| Method declarations |    void method()    |    No    |    Anywhere inside a class    |

- When there  are imports with wildcard `*`, the compiler will get only the classes he needs.
- If there is more than one class in a file, only one **must** be public. The others inner class don’t have scope.
- Advice for question about garbage collector and references : draw on a paper the situation with squares and arrows, it will be easier.
- `finalize()` call could run zero or one time.

----------------
- **SUMMARY p.86 (40)**
- **Exam essentials p.87 (41)**

----------------

**[Back to top](#sommaire)**

## Chapter 2 Operators and Statements

- Arithmetic have the same precedence than in mathematics. In this operation `int x = 2 * 5 + 3 * 4 - 8;` the multiplicative operators will be evaluated in first.
- The modulus operator `%` gives the remainder of the operation. `11%3` gives 2.
- The JVM prefers to work with `int` and `double` value. So if it is possible, it will work with this type of data.
- Be careful with `++` and `--`

	```java
	int y = 1;
	System.out.println(y--); //Show 1
	System.out.println(y); //Show 0
	
	int x = 10
	System.out.println(++x); //Show 11
	```
- Operators `++` and `--` are evaluated directly in the expression
	
	```java
	int x = 3;
	int y = ++x * 5 / x-- + --x;
			  4       4       2 (3 after the + and 2 with --)
	(int y = 4 * 5 / 4 + 2)		  
	System.out.println("x is " + x); //Show 2
	System.out.println("y is " + y); //Show 7 
	```
- Be careful with compound assignment operator, with this one the cast is automatic. 
	
	```java
	long x = 10;						long x = 10; 
	int y = 5;							int y = 5;
	y = y * x; 							y *= x;
	// DOES NOT COMPILE					//COMPILE
	```
- Be careful with logical operators `&&` and `&`. Look at these cases :
	
	```java
	X x = null;
	if(x != null && x.getValue() < 5) { 
		// COMPILE because the second expression is not evaluated
	}
	if(x != null & x.getValue() < 5) { 
		// Throws an exception if x is null because the second condition is evaluated	
	}
	```
- `==`	with objects is related to references, if you want to compare the equality of objects they are pointing on, use `.equals()`.
- Be careful with this one
	
	```java
	if(hourOfDay < 11) 
		System.out.println("Good Morning"); 
		morningGreetingCount++;
	``` 
	`morningGreetingCount++;` will be executed every time. If there are no brakes after `if` statement, only the next line will be considered like a part of it.
- Be careful with this case
	
	```java
	int y = 1;
	int z = 1;
	final int x = y<10 ? y++ : z++; 
	System.out.println(y+","+z); // Outputs 2,1 only the true condition will be evaluated
	```
- Only this types are allowed in `switch` statements 
	- int and Integer
	- byte and Byte
	- short and Short
	- char and Character 
	- String
	- enum values
- Important quote : **There is no requirement that the case or default statements be in a particular order, unless you are going to have pathways that reach multiple sections of the switch block in a single execution.** See p. 121 for examples.
- In a for declaration, the variable initialized must be of the same type. 

--------------
- **Summary p.138 (92)**
- **Exam essentials p.138 (92)**

----------------

**[Back to top](#sommaire)**

## Chapter 3 Core Java APIs

### String

- Be careful with `String` objects, they are immutable. Once it is created, it is not allowed to change
	
	```java
	String s1 = "1";
	String s2 = s1.concat("2"); 
	s2.concat("3"); 
	System.out.println(s2); //Print 12 dans not 123
	```
- String index are the same then arrays
- Be careful with the `substring(int indexOfStart, int indexOfEnd)` method. The `indexOfEnd` is not include, it is a stop.
	
	```java
	String b = "animals";
	System.out.println(string.substring(3, 4)); // m
	```

### StringBuilder

- `StringBuilder` is mutable and be careful because it returns a reference of itself
	
	```java
	StringBuilder a = new StringBuilder("abc"); 
	StringBuilder b = a.append("de"); //Returns a reference of itself so b is pointing as the same object as a
	b = b.append("f").append("g"); 
	System.out.println("a=" + a); //Print abcdefg
	System.out.println("b=" + b); //Print abcdefg
	```
- `delete()` method as the same rule than `substring()`

	```java
	StringBuilder sb = new StringBuilder("abcdef");
	sb.delete(1, 3);
	System.out.println(sb); // adef
	```
- Be careful with equality of `String` and `StringBuilder`

	```java
	StringBuilder one = new StringBuilder(); 
	StringBuilder two = new StringBuilder(); 
	StringBuilder three = one.append("a"); //Returns a reference of itself
	System.out.println(one == two); // false 
	System.out.println(one == three); // true
	```
	```java
	String x = "Hello World";
	String y = "Hello World"; 
	System.out.println(x == y); // true because of the string pool
	```
	```java
	String x = "Hello World";
	String z = " Hello World".trim(); // A new object is created
	System.out.println(x == z); // false
	```
	```java
	String x = new String("Hello World"); 
	String y = "Hello World"; 
	System.out.println(x == y); // false
	```
	```java
	String x = "Hello World";
	String z = " Hello World".trim(); 
	System.out.println(x.equals(z)); // true because it uses equals methèè
	```
- The ``substring()`` method doesn't affect the value of a Stringbuilder. 

### Arrays and ArrayList

- Be careful with array declaration	

	```java
	int[] ids, types; // two arrays
	int ids[], types; // one array and one int
	int[] vars3[]; // 2D array
	int[] vars4 [], space [][]; // a 2D AND a 3D array
	```
- Binary search rules
	
|                 Scenario                 |                                                           Result                                                           |
|:----------------------------------------:|:--------------------------------------------------------------------------------------------------------------------------:|
|   Target element found in sorted array   |                                                       Index of match                                                       |
| Target element not found in sorted array | Negative value showing one smaller than the negative of index, where a match needs to be inserted to preserve sorted order |
|              Unsorted array              |                                          A surprise—this result isn’t predictable                                          |

- For multi-dimensional array, it is recommended to draw it on a paper
- Since java 7, adding the type in diamonds operators `<>` is not required for `ArrayList` declaration
- When you create an `ArrayList` from an array the `ArrayList` the two elements are linked and the size of the `ArrayList` is fixed. So you cannot remove an element of the `ArrayList`, and when you change an element of one it changes in the other

### Wrapper classes and auto boxing

- Be careful with this example, and it is the same for the other primitive except the `Character` that doesn't have these methods

	```java
	int primitive = Integer.parseInt("123"); 
	Integer wrapper = 	Integer.valueOf("123");
	```
	Because au auto boxing you can permute the declarations.
- Be careful with this tricky one

	```java
	List<Integer> numbers = new ArrayList<>(); 
	numbers.add(1);
	numbers.add(2);
	numbers.remove(1); 
	System.out.println(numbers); //Prints [1]
	```
	Because there’s already a `remove()` method that takes an `int` parameter, Java calls that method rather than autoboxing.

### Dates and Times

- The implementation of `Date` in java 7 is **not** in the exam.
- The classes `LocalDate`, `LocalDateTime`, `LocalTime` are immutable.
- There is no constructor for `LocalDate`, `LocalDateTime`, `LocalTime`. You have to use the static methods.
- Methods on Period allow chaining but only the laste is effective.

----------------
- **SUMMARY p.197 (151)**
- **Exam essentials p.198 (152)**

----------------

**[Back to top](#sommaire)**

## Chapter 4 Methods and Encapsulation

### Designing Methods

- Method declaration : 
	- Elements required : 
		- `void` (type), 
		- name, 
		- parenthesis for `parameter` but **can be empty**
		-  brackets for `method body` but **can be empty**.
	- Others elements : 
		- access modifier
- Access method modifiers are identical of class' modifiers.
- The modifiers are special but are not in the exam : `synchronized`, `native`, `strictfp`.
- Be careful with the order of modifiers, it has an importance, example :

	```java
	public void walk1() {}
	public final void walk2() {}
	public static final void walk3() {}
	public final static void walk4() {}
	public modifier void walk5() {} // DOES NOT COMPILE 
	public void final walk6() {} // DOES NOT COMPILE 
	final public void walk7() {}
	```
- Methods' name follow the same rules of variable's names.

### Working with Varargs

- `Varargs` parameters must appear at the end of the parameters list and at least must be unique. It is optional so these different declarations are valid 
		
	```java
	public void method(int ... args){}; // Declaration
	method() // Empty array
	method(1, 2) // Array with two elements
	method(new Integer[]{1,2}) // Array with two elements
	```

### Applying Access Modifiers	

- Be careful with this example
	
	```java
	package pond.shore; 
	public class Bird {		protected String text = "floating"; 
		protected void floatInWater() {			System.out.println(text); 
		}
	}
	
	package pond.swan;
	import pond.shore.Bird; // in different package than Bird
	
	public class Swan extends Bird { // but subclass of bird
		public void swim() {
			floatInWater(); // package access to superclass
			System.out.println(text); // package access to superclass
		}
		public void helpOtherSwanSwim() {
			Swan other = new Swan(); //we are in the class
			other.floatInWater(); // package access to superclass
			System.out.println(other.text); // package access to superclass
		}
		public void helpOtherBirdSwim() {
			Bird other = new Bird();
			other.floatInWater(); // DOES NOT COMPILE
			System.out.println(other.text); // DOES NOT COMPILE
		}
	}
	```
	Lines 24 and 25 don't compile because `Bird` reference is not in the same package and is not a subclass of `Bird` so it doesn't compile. It is important to verify if we are in the same package of the reference we use or if this reference extends the class we want to use methods.
- Summary of access modifiers :

	| Can access | Private ? | Default ? | Protected ? | Public ? |
	|:--------------------------------------------------------------:|:--------------------------:|:----------------------------------------------------:|:----------------------------:|:-------------------------:|
	| Member in the same class | Yes | Yes | Yes | Yes |
	| Member in another class in same package | No | Yes | Yes | Yes |
	| Member in a superclass in a different package | No | No | Yes | Yes |
	| Method/field in a non- superclass class in a different package | No | No | No | Yes |
	
### Designing Static Methods and Fields

- Static fiels are shared with every instance of the class.
- Be careful with this one
	
	```java
	public class Koala{
		public static count = 0;
		public static void one(){};
		public static void two(){};
		public void three(){};
		public static void main(String[] args){
			System.out.println(count);
		}
	}
	public class KoalaTester{
		public static void main(String[] args){
			Koala k = new Koala();
			System.out.println(k.count); // k is a Koala
			k = null;
			System.out.println(k.count); // k is still a Koala
		}
	}
	```
	Because we refer to `count` that is a static field, the instance is not important !
- **A static member cannot call an instance member.**
- `final` makes the compiler check that you are not assign the variable to another value or reference. For objects, you can apply methods on them but not assign them to another reference. For primitive, their value can't change.
- For `final` members, if there is not assignation at the first time, an assignation can be realized in a static bloc, example :
	
	```java
	private static int one;
	private static final int two;
 	private static final int three = 3;
 	private static final int four; // DOES NOT COMPILE
 	static {
		one = 1;
		two = 2;
		three = 3; // DOES NOT COMPILE
 		two = 4; // DOES NOT COMPILE
	}
	```
- For static import, you can only import methods and not classes. And be careful with methods with same name, even they are in different packages.

	```java
	import static statics.A.TYPE;	import static statics.B.TYPE; // DOES NOT COMPILE
	```

### Passing Data Among Methods
	
- In Java, parameters of the methods are passed by copy. If you do a reassignement in it, the variable won't change. Only methods called on a object will change it.

### Overloading Methods

- The overloading of methods is only possible by changing parameters but not type of return. Be careful with varargs parameters because Java consider them as an array, so look at this one : 
	
	```java 
	public void fly(int[] lengths) { }
	public void fly(int... lengths) { } // DOES NOT COMPILE
	```
- If there are these two methods : `public void fly(int numMiles) { }` and `public void fly(Integer numMiles) { }`. If Java sees an int as parameter it won't do autoboxing and use the first one. It is a "lazy" mode. 
- Be careful with this one, it cannot handle converting in two steps to a long and then to a Long :
	
	```java
	public class TooManyConversions {
  		public static void play(Long l) { }
  		public static void play(Long... l) { }
  		public static void main(String[] args) {
    		play(4); // DOES NOT COMPILE
    		play(4L); // calls the Long version
  		} 
  	}
	```

### Creating Constructors
	
- The calling of `this` to reference another constructor must be the first call even if it does not compile. 
- `final` fields must be initialized at least In the constructor.
- Order of initialization :
	1. If there is a superclass, initialize it first (we’ll cover this rule in the next chapter. For now, just say “no superclass” and go on to the next rule.)
	2. Static variable declarations and static initializers in the order they appear in the file
	3. Instance variable declarations and instance initializers in the order they appear in the file.
	4. The constructor.
- Be very careful with the order of initialization :
	
	```java
	public class YetMoreInitializationOrder {
		static { add(2); }
		static void add(int num) { System.out.print(num + " "); }
		YetMoreInitializationOrder() { add(5); }
		static { add(4); }
		{ add(6); }
		static { new YetMoreInitializationOrder(); }
		{ add(8); }
		public static void main(String[] args) { }
	}	 
	```
	This one print : 2 4 6 8 5. First static blocs then bloc of constructor and finaly constructor. 
- Calling of methods has this order : exact matches are used first, followed by wider primitives, followed by autoboxing, followed by varargs.

----------------

----------------

**[Back to top](#sommaire)**

## Chapter 5 Class Design

- Java disallow multiple inheritance.
- For the OCA exam, you should only be familiar with public and default package-level class access modifiers.
- Beware with this example, there is not problem with this file :
	
	```java
	class Rodent {}
	public class Groundhog extends Rodent {}
	```
	But if we add `public` to class `Rodent` there will be a compilation error
- The `super()` instruction used in constructor to call parent's one **must be only at the first line !**
- Java automatically insert reference to super constructor if the parent class doesn't have a constructor written. Example :
	
	```java
	public class Mammal { public Mammal(int age) { }
	}
	public class Elephant extends Mammal { 
	// DOES NOT COMPILE 
	}
	----------
	public class Mammal { 
		public Mammal(int age) { 
		}
	}
	public class Elephant extends Mammal { 
		public Elephant() { 
			// DOES NOT COMPILE 
		}
	}
	-----------
	public class Mammal { 
		public Mammal(int age) { 
		}
	}
	public class Elephant extends Mammal { 
		public Elephant() {
			super(10);
			//No Problems
		}
	}
	```
- Constructor Definition Rules:
	1. The first statement of every constructor is a call to another constructor within the class using this(), or a call to a constructor in the direct parent class using super().
	2. The super() call may not be used after the first statement of the constructor.
	3. If no super() call is declared in a constructor, Java will insert a no-	argument super() as the first statement of the constructor.
	4. If the parent doesn’t have a no-argument constructor and the child doesn’t define any constructors, the compiler will throw an error and try to insert a default no-argument constructor into the child class.
	5. If the parent doesn’t have a no-argument constructor, the compiler requires an explicit call to a parent constructor in each child constructor.
- If there are no conflicts on the name of variable, the using of this in the child class will call public, package or protected member of the parent class.
- The compiler performs the following checks when you override a non private method:
	1. The method in the child class must have the same signature as the method in the parent class.
	2. The method in the child class must be at least as accessible or more accessible than the method in the parent class.
	3. The method in the child class may not throw a checked exception that is new or broader than the class of any exception thrown in the parent class method.
	4. If the method returns a value, it must be the same or a subclass of the method in the parent class, known as covariant return types. 
- Important rule for difference and check between overloading and overriding : **Any time you see a method that appears to be overridden on the example, first check to make sure it is truly being overridden and not overloaded. Once you have confirmed it is being overridden, check that the access modifiers, return types, and any exceptions defined in the method are compatible with one another.**
- Be careful with exceptions thrown by methods with inheritance (see p 250 for examples).
- If a method is `private` in the parent, no rules applied for overriding or overloading it in the child.
- Rules for static methods :
	1. The method in the child class must have the same signature as the method in the parent class.
	2. The method in the child class must be at least as accessible or more accessible than the method in the parent class.
	3. The method in the child class may not throw a checked exception that is new or broader than the class of any exception thrown in the parent class method.
	4. If the method returns a value, it must be the same or a subclass of the method in the parent class, known as covariant return types.
	5. The method defined in the child class must be marked as static if it is marked as static in the parent class (method hiding). Likewise, the method must not be marked as static in the child class if it is not marked as static in the parent class (method overriding).
- **Be careful with methods overriding. If parent's method is static, it will be his own that will be used but if it is not, the child's one will override it.**
- Be careful with abstract methods. Examinators will presente some of them with body statement and if it's the case, the code won't compile !
- Abstract class definition rules:
	1. Abstract classes cannot be instantiated directly.
	2. Abstract classes may be defined with any number, including zero, of abstract and non-abstract methods.
	3. Abstract classes may not be marked as private or final.
	4. An abstract class that extends another abstract class inherits all of its abstract methods as its own abstract methods.
	5. The first concrete class that extends an abstract class must provide an implementation for all of the inherited abstract methods.
- Abstract method Definition rules:
	1. Abstract methods may only be defined in abstract classes.
	2. Abstract methods may not be declared private or final.
	3. Abstract methods must not provide a method body/implementation in the abstract class for which is it declared.
	4. Implementing an abstract method in a subclass follows the same rules for overriding a method. For example, the name and signature must be the same, and the visibility of the method in the subclass must be at least as accessible as the method in the parent class.
- Interfaces rules 
	1. Interfaces cannot be instantiated directly.
	2. An interface is not required to have any methods.
	3. An interface may not be marked as final.
	4. All top-level interfaces are assumed to have public or default access, and they must include the abstract modifier in their definition. Therefore, marking an interface as private, protected, or final will trigger a compiler error, since this is incompatible with these assumptions. 
	5. All nondefault methods in an interface are assumed to have the modifiers abstract and public in their definition. Therefore, marking a method as private, protected, or final will trigger compiler errors as these are incompatible with the abstract and public keywords.
- Multiple inheritance is allowed for interfaces
- Be careful with traps in the exam with this rule : a class can implement an interface, a class cannot extend an interface. Likewise, whereas an interface can extend another interface, an  interface cannot implement another interface.
- Rules for the new `default` keyword used for interfaces 
	1. A default method may only be declared within an interface and not within a class or abstract class.
	2. A default method must be marked with the default keyword. If a method is marked as default, it must provide a method body.
	3. A default method is not assumed to be static, final, or abstract, as it may be used or overridden by a class that implements the interface.
	4. Like all methods in an interface, a default method is assumed to be public and will not compile if marked as private or protected.
- If a class implements two interfaces that have default methods with the same name and signature, the compiler will throw an error. There is an exception to this rule, though: if the subclass overrides the duplicate default methods, the code will compile without issue—the ambiguity about which version of the method to call has been removed.
- Here are the static interface method rules you need to be familiar with:
	1. Like all methods in an interface, a static method is assumed to be public and will not compile if marked as private or protected.
	2. To reference the static method, a reference to the name of the interface must be used.
- A class that implements two interfaces containing static methods with the same signature will still compile at runtime. 
- Summary of polymorphism rules 
	1. The type of the object determines which properties exist within the object in memory.
	2. The type of the reference to the object determines which methods and variables are accessible to the Java program.

## Chapter 6 Exceptions

----------------
----------------

**[Back to top](#sommaire)**
