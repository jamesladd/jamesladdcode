---
layout: post
title: DRAFT A Design Compass East Oriented
tags: east
---
DRAFT - A Design Compass: East Oriented is an article I put together to explore a different way of structuring code for better results. An editor I showed it to said that I had to dumb it down and make it less conversational, so I thought I would post the draft here for your comment.

#### A Design Compass: East Oriented

> Procedural code gets information then makes decisions.<br>
> Object-oriented code tells objects to do things.<br>
> - Alec Sharp


Structuring of code to an East orientation decreases coupling and the amount of code needed to be written, whilst increasing code clarity, cohesion and flexibility. It is easier to create a good design and structure by simply orienting it East.

#### Introduction
So you want to design, write and refactor code like a seasoned pro but are unsure of what direction to take and how to navigate around pitfalls and obstacles to get the best results? It would be great if there was a design compass that showed the way to a code shangrila where inversion of control, loose coupling, testability, reuse and more are bountiful.

A design compass does exist and this article will show you how to obtain and use the compass to achieve code that leaves you with time to do other things or just enjoy the tranquility and confidence that comes from having great code that does more. Seasoned professionals may also put the compass to use in finding hidden gems of functionality and dependencies that can be inverted or to help direct design or testing efforts towards a more fruitful result, but whatever your level, a compass can certainly come in handy in getting from place A to B.

#### The Compass 
Objects can flow in many directions and the design and structure of the code dictates these directions. The four points of the compass can be applied to the structure and execution of program code. When looking at the code on paper or screen you find North is up a layer, South is down a layer, West is left away from an object and East is right towards another object. When you make a call into a method you go deeper into the code or down and so you can say you are going South. Similarly when execution returns up to the caller you can say you are going North. Within a method, execution typically travels South with the interaction between that method and other methods and variables providing the West and East points. When you call a method and that method takes one or more arguments it can be said that the arguments are travelling East away from the caller and into the method. When a method returns a result, that result travels West away from the called method. In most cases the structure and typical execution of code is South with many East and West movements.

#### East Oriented
Now that we have a compass, what remains is how to use it to navigate to a better design. Luckily our compass is very easy to use as only one compass direction need be followed and that direction is East. The East approach is the structuring of code to an East orientation. The rigorous application of this East principle decreases coupling and the amount of code that needs to be written, whilst increasing the clarity, cohesion, flexibility, reuse and testability of that code. It is easier to create a good design, structure and identify dependencies and abstractions by simply orienting East. The remainder of this article is a journey East through some examples with stops along the way to explain the benefits found on this path.

#### Eastward Bound
*something more here (I said it was a draft)*

The code in *Example 1* moves the account instance's balance object West into the local variable balance. This movement West couples the code with the representation of balance, requires time and resources to assign and hold the balance and increases the lines of code needed in the example. In this scenario the method getBalance() does little to convey the intent or behaviour required of the account or balance objects.

The code in *Example 2* describes an intent to move the account instances balance object East towards another object. This movement East increases sequential cohesion and decouples the code with the representation of balance as only the Account Class knows the representation. No resources and time are used to hold the balance and the method putBalance(PrintWriter) conveys more intent about the use of a balance. There is also less code to write, maintain and comprehend.

The code in *Example 3* is an improvement on *Example 1* as it reduces coupling and doesn't use extra resources and time to hold the balance object. However, it still requires the Account Class to expose a balance that can travel West and to represent a balance in the limited forms acceptable to the type of System.out objects.

**Example 1.**
{% highlight java %}
balance = account.getBalance();
System.out.println(balance);
{% endhighlight %}

Moves 'balance' object West.

**Example 2.**
{% highlight java %}
account.putBalance(System.out);
{% endhighlight %}

Moves 'balance' object East.

**Example 3.**
{% highlight java %}
System.out.println(account.getBalance());<br />
{% endhighlight %}

Moves 'balance' object West.


Structuring a Class to support only East bound objects provides more flexibility in deciding the internal representation of a Class' data and when that representation must be made concrete for use by an external Class. The contrasts in internal representations and flexibility is shown in *Examples 4* to *7*. Note that the changes between *Example 4* and *Example 5* would require all the places in the application that call 'getBalance()' to be updated, whilst the changes shown in *Example 6* and *Example 7* would not.

**Example 4.**
{% highlight java %}
BigDecimal getBalance() {
    return balance;
}
{% endhighlight %}

**Example 5.**
{% highlight java %}
String getBalance() {
    return balanceIntegralPart + '.' + balanceDecimalPart;
}
{% endhighlight %}

**Example 6.**
{% highlight java %}
putBalance(Writer writer) {
    writer.write(balanceIntegralPart);
    writer.write('.');
    writer.write(balanceDecimalPart);
}
{% endhighlight %}

**Example 7.**
{% highlight java %}
putBalance(Writer writer) {
    writer.write(balance);
}
{% endhighlight %}

Structuring a Class to support only East bound objects enables the West side of the conversation to be more generic since there is less coupling and you simply don't need to store transient variables. There will always be arguments that not orienting things East is simpler. However, this is not the case if the full development lifecycle is considered along with enhanced reuse. When applying the principle again and again it becomes second nature and quicker.

Martin Fowler (2004) said "To help make all of this more concrete I'll use a running example to talk about all of this. Like all of my examples it's one of those super-simple examples; small enough to be unreal, but hopefully enough for you to visualize what's going on without falling into the bog of a real example." and I will do likewise, using his example, which we will refactor using the East principle.

**Example 8.**
{% highlight java %}
class MovieLister...

public Movie[] moviesDirectedBy(String arg) {
    List allMovies = finder.findAll();
    for (Iterator it = allMovies.iterator(); it.hasNext();) {
        Movie movie = (Movie) it.next();
        if (!movie.getDirector().equals(arg)) it.remove();
    }
    return (Movie[]) allMovies.toArray(new Movie[allMovies.size()]);
}
{% endhighlight %}

The MovieLister class makes several assumptions and misses a few opportunities which can be gained by orienting it East. It is assumed the users of MovieLister can use an Array of Movie objects, and the implementation iterates through the list of movies twice, once to read all the movies and a second time to filter out the movies that are not by the required Director. To start with lets orient the implementation East without changing the signature of the 'moviesDirectedBy' method.

**Example 9.**
{% highlight java %}
class MovieLister...

public Movie[] moviesDirectedBy(String arg) {
    List allSelectedMovies = new ArrayList();
    finder.findAllSelectingTo(selectDirector(arg), allSelectedMovies);
    return (Movie[]) allSelectedMovies.toArray(new Movie[allSelectedMovies.size()]);
}

private Selector selectDirector(final String arg) {
    return new Selector() {
        public boolean isSelected(Object object) {
            return ((Movie) object).getDirector().equals(arg);
        }
    ...
{% endhighlight %}

In *Example 9* the core finding of the movies by a director has been oriented East, with the 'findAll' method changed to 'findAllSelectingTo'. This method is read as 'Find All, Selecting those that match and adding them To the list'. A benefit of allowing the finder to find and select is that it now has the opportunity to apply the selector at a time that is optimal for it. This opportunity did not exist before.

In the original MovieLister (Fowler 2004) the list of movies is read from a colon delimited file and in that instance the selector could be applied as each movie is read, removing the need to iterate through the list a second time. The creation of the Selector functor on each call can be removed to a static constant and is shown this way for brevity. Passing a list to the finder enables the user of the finder to have some say in how the results are stored which may remove the need to convert the results list. If the need arises to find movies of a specific genre then adding a new selector, 'selectGenre' is simple with the bulk of the code reused. Applying the East principle naturally drives the design to be more flexible and increases reuse as shown in this example. Some readers may ask why the method 'isSelected' has a boolean that travels West, and this will be removed in the next step.

Our journey East is not yet complete as more benefits can be obtained from enhancing the 'moviesDirectedBy' method further. In this next step we will enable the caller of the 'moviesDirectedBy' method to specify where the output of the MovieLister should go and remove the West traveling boolean introduced by the 'isSelected' method.  Typically the object returned from a method will be used for some purpose, and in Fowler (2004) the use of the method result is unclear. For our purposes we assume that the list of movie objects found by the MovieLister is to be output onto the console and alternatively written to a file.

**Example 10.**
{% highlight java %}
class MovieLister...

public void applyToTheMoviesDirectedBy(
                final MovieAction movieAction,
                final String director) {
   
     finder.findAllAndApply(
         new MovieAction() {
              public void applyTo(Movie movie){
                  movie.ifDirectedByDo(director, movieAction);
              }
         }
    );
}
{% endhighlight %}

A lot has happened in Example 10, the least of which is the renaming of methods to names that convey more intent. Using concepts described by Robert Cecil Martin (2005) the MovieLister has been reworked with dependencies injected as interfaces, such as Movie and MovieAction. There is less code in the implementation and it is more flexible with any action able to be applied to the list of movies filtered by director. Example 11 shows the refactored code being called to output movies to the console and to a file. A new destination for the movies, like a web page, can be added by just introducing a new MoviesClient and reusing all the other code. All the dependencies are now nicely inverted so the code can be made simpler using an IOC container like Spring but this is left as an exercise for you.

**Example 11.**
{% highlight java %}
class Main...

movieLister.applyToTheMoviesDirectedBy(
            new ExampleAddMovieAction(new ExampleMoviesClientConsoleAdaptor(System.out)),
            director);

movieLister.applyToTheMoviesDirectedBy(
            new ExampleAddMovieAction(new ExampleMoviesClientFileAdaptor("movies.txt")),
            director);
{% endhighlight %}

#### A New Path
Some colleagues have commented on similarities to the 'Hollywood' and 'Tell, Dont Ask' principles but the East principle is different. The 'Hollywood' principle describes an event driven model and such a framework could ask an object for a value, something the East principle would not allow, since the value would travel West. However, there is a similarity in the use of constructs like closures and callbacks to achieve results. The East principle is not like the 'Tell, Dont Ask' principle because there is no ambiguity, you simply cannot ask, you can only tell. As Abhijit Hiremagalur pointed out, some people think I'm telling the object to give me a value which is where the ambiguity comes into the 'Tell, Dont Ask' principle. The East principle requires that all public methods are void (do not return a value) or return the receiver (this in Java or self in Smalltalk) without exception.

#### Conclusion
Ill add to this as soon as I get some more feedback

#### References:
[Fowler, Martin (2004), Inversion of Control Containers and the Dependency Injection pattern](http://martinfowler.com/articles/injection.html)<br>
[Fowler, Martin (2005), Inversion of Control](http://www.martinfowler.com/bliki/InversionOfControl.html)<br>
[Martin, Robert Cecil (2005), The Dependency Inversion Principle](http://www.objectmentor.com/resources/articles/dip.pdf)<br>
[Martin, Robert Cecil (2002) Single Responsibility Principle, Agile Software Development: Principles, Patterns, and Practices](http://www.objectmentor.com/resources/articles/srp.pdf)<br>
[Appleton, Brad, Introducing Demeter and its Laws](http://www.cmcrossroads.com/bradapp/docs/demeter-intro.html)<br>
Sharp, Alec (1997), Smalltalk by Example, McGraw-Hill

[East Example Code]({{ site.url }}/public/uploads/east-example.zip)





