##  Design pattern ##

 Design patterns are standard solutions/technique for commonly (and frequently) occurred problems that software developers faced during software development.

## Gang of Four (GOF) ##

 Erich Gamma, Richard Helm, Ralph Johnson and John Vlissides published a book titled Design Patterns - Elements of Reusable Object-Oriented Software which initiated the concept of Design Pattern in Software development.In this book,there are 23 design patterns which can be classified in three categories:
 1. <b>Creational</b>:
 If object creation code(using new opreator - traditional way to create object) is spread in whole application, and if you need to change the process of object creation e.g new constructor is used then you need to go in each and every place to make necessary changes . As a result application manages become more difficult and complicated,need to more testing effort.
 Following design pattern gives application more flexibility and smart way to create object
 	1. Factory(Factory Method) pattern 
 	2. Abstract Factory pattern 
 	3. Singleton pattern 
 	4. Prototype Pattern 
 	5. Builder pattern 
 	6. Object Pool Pattern 
 2. <b>Structural</b>:
 These design patterns concern class and object composition. Concept of inheritance is used to compose interfaces and define ways to compose objects to obtain new functionalities.
 	1. Adapter pattern 
 	2. Bridge pattern 
 	3. Filter pattern 
 	4. Composite Pattern 
 	5. Decorator pattern 
 	6. Facade Pattern  
 	7. Flyweight Pattern  
 	8. proxy Pattern  
 3. <b>Behavioral pattern</b>:
 These design patterns are specifically concerned with communication between objects.
 	1. chain of responsibility pattern 
 	2. Command pattern 
 	3. Interpreter pattern 
 	4. Iterator Pattern 
 	5. Mediator pattern
 	6. Observer pattern
 	7. State pattern
 	8. Null Object pattern
 	9. Strategy pattern

## Factory Method pattern ## 

Factory, as name suggest, is a place to create some different products which are somehow similar in features yet divided in categories.
Programatically, factory pattern have creational methods that return instances of different classes of abstract/interface type.

## Use case ##

Factory pattern is most suitable where there is some complex object creation steps are involved and may be changed further.

## Use at jdk ###
1. java.util.Calendar#getInstance()
2. java.util.ResourceBundle#getBundle()
3. java.util.EnumSet#of()
4. javax.xml.bind.JAXBContext#createMarshaller()
5. java.lang.Class#forName()

```java
public abstract class Calendar implements Serializable, Cloneable, Comparable<Calendar> {
    /**
      * Gets a calendar using the default time zone and locale. The
      * <code>Calendar</code> returned is based on the current time
      * in the default time zone with the default locale.
      *
      * @return a Calendar.
      */
     public static Calendar getInstance()
     {
         Calendar cal = createCalendar(TimeZone.getDefaultRef(), Locale.getDefault(Locale.Category.FORMAT));
         cal.sharedZone = true;           
          return cal;
     }

     private static Calendar createCalendar(TimeZone zone,
                                              Locale aLocale)
        {
            Calendar cal = null;
  
            String caltype = aLocale.getUnicodeLocaleType("ca");
            if (caltype == null) {
                // Calendar type is not specified.
                // If the specified locale is a Thai locale,
                // returns a BuddhistCalendar instance.
                if ("th".equals(aLocale.getLanguage())
                        && ("TH".equals(aLocale.getCountry()))) {
                    cal = new BuddhistCalendar(zone, aLocale);
                } else {
                    cal = new GregorianCalendar(zone, aLocale);
                }
            } else if (caltype.equals("japanese")) {
                cal = new JapaneseImperialCalendar(zone, aLocale);
            } else if (caltype.equals("buddhist")) {
                cal = new BuddhistCalendar(zone, aLocale);
            } else {
                // Unsupported calendar type.
                // Use Gregorian calendar as a fallback.
                cal = new GregorianCalendar(zone, aLocale);
            }
    
            return cal;
       }
}
```
Create an interface or abstract class
```java
package com.javaaround.dpattern.factorypattern;
public interface Shape {
   void draw();
}
```



Create implementation classes implementing the same interface.

```java
package com.javaaround.dpattern.factorypattern;
public class Square implements Shape {

   @Override
   public void draw() {
      System.out.println("This is square draw() method.");
   }
}
```

```java
package com.javaaround.dpattern.factorypattern;
public class Rectangle implements Shape {

   @Override
   public void draw() {
      System.out.println("This is Rectangle method.");
   }
}
```

```java
package com.javaaround.dpattern.factorypattern;
public class Circle implements Shape {

   @Override
   public void draw() {
      System.out.println("This is Circle method");
   }
}
```



Create Factory class to get instance of above class

```java
package com.javaaround.dpattern.factorypattern;
public class ShapeFactory {
	
 
   //use getShape method to get object of type shape 
   public static Shape getShape(String shapeType){
   	  if(shapeType == null)
         return null;	
      if(shapeType.equalsIgnoreCase("CIRCLE"))
         return new Circle();
         
      else if(shapeType.equalsIgnoreCase("RECTANGLE"))
         return new Rectangle();
         
      else if(shapeType.equalsIgnoreCase("SQUARE"))
         return new Square();
      
      
      return null;
   }
}   
```
Usage : <br>

```java
package com.javaaround.dpattern;
import com.javaaround.dpattern.factorypattern.ShapeFactory;
import com.javaaround.dpattern.factorypattern.Shape;

/**
 * Hello world!
 *
 */
public class App 
{
    public static void main( String[] args )
    {
    	System.out.println("Hello World");
    	
      //get shape by providing param
      Shape shape = ShapeFactory.getShape("CIRCLE");
      //display
      shape.draw(); 

    }
}

```

## Abstract Factory pattern ## 
Provide an interface for creating families of related or dependent objects without specifying their concrete classes. A creational methods returning the factory itself which in turn can be used to create another abstract/interface type
It is considered as another layer of abstraction over factory pattern

### JDK Usage ###
1. javax.xml.parsers.DocumentBuilderFactory#newInstance()
2. javax.xml.transform.TransformerFactory#newInstance()
3. javax.xml.xpath.XPathFactory#newInstance()