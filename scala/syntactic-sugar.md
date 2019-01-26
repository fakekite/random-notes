Anonymous class instantiations

Reference: [link](https://scalerablog.wordpress.com/2015/05/29/seeking-anonymity-anonymous-classes/
)

Magic

```scala
trait AnonymousHero {
  def superpower: String
}
 
// An anonymous class is instantiated directly from the trait
val myHero = new AnonymousHero {
  def superpower = "I can compile Scala with my brain"
}
```

Under the hood

```scala
// A concrete class is still declared with the same implementation provided above.
// Note the name `AnonymousHeroClass` is made up by the writer so the explanation
// is easy to understand. I do not really know what name scalac assigned.
// TODO figure out whether scalac assigned a random name or not.
class AnonymousHeroClass extends AnonymousHero {
  def superpower: String = "I can compile Scala with my brain"
}
 
val myHero = new AnonymousHeroClass
```

Value added

`Program to interface` becomes the only choice now. e.g. 

```scala
object AnonymousHero {
  // Hide the implementation behind the factory method
  def apply() = new AnonymousHero {
    def superpower = "I can compile Scala with my brain"
    
    // A method defined in the implementation
    def complain = "I need a break"
  }
}

// ....

val hero = AnonymousHero()
hero.complain // This will not compile since `complain` is not defined in the trait
```


Trivia

This topic should not be confused with Java's [anonymous class](https://docs.oracle.com/javase/tutorial/java/javaOO/anonymousclasses.html).





