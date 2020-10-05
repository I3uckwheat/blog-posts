# Strategy Pattern

Design patterns are a non-fundamental piece of software engineering. They are named and used not by their concrete examples, but by the patterns that have been developed over time. They are used to help write cleaner, and more extensible code. This pattern facilitates composition over inheritance, which is considered good practice. 

The **Strategy Pattern** is often used to replace chains of "if, else if, else" statements within classes. Whenever you identify a place where you're using different methods of processing data within the same context you can also use this pattern to help make it easier to add more ways of processing that data. For example, you may have different algorithms with different time or space complexity trade-offs to choose from.

## What is it?

The **Strategy Pattern** is a behavioral pattern used to encapsulate a family of algorithms (or behaviors) in a way that can be used interchangeably. These encapsulations are called *strategies*. Before reading on, please take a quick look at [this uml diagram](https://upload.wikimedia.org/wikipedia/commons/4/45/W3sDesign_Strategy_Design_Pattern_UML.jpg) and keep it in mind. 

Implementing the strategy pattern requires a few levels of organization, each strategy needs a common interface for each strategy; this is commonly an abstract class, but can also be classes with identical APIs in weakly typed languages. Next, a strategy needs a way to receive information from the application, this is done via a *context* class. The context class is the messenger between the strategy, and the rest of the application. 

Below is some code that outlines a few of the concepts on this pattern.

~~~javascript
// This is the 'context'
class Grader { 
	constructor(gradingStrategy, possibleScore, allScores) {
		this.gradingStrategy = gradingStrategy;
		this.possibleScore = possibleScore;
		this.allScores = allScores;
	}

	getGrade(studentScore) {
		return this.gradingStrategy.grade(
			this.possibleScore,
			studentScore,
			this.allScores
		);
	}
}

// Abstract class, not *really* needed in JS, but strongly typed languages will require this as an interface
class GradeStrategy { 
	constructor() {
	}

	grade(possibleScore, studentScore, allScores) {
    throw new Error("grade not implemented");
  }
}

// Strategy
class PercentGradeStrategy extends GradeStrategy {
	constructor() {
		super();
	}

	grade(possibleScore, studentScore) {
		return studentScore / possibleScore;
	}
}

// Strategy
class LetterGradeStrategy extends GradeStrategy {
	constructor() {
		super();
	}

	grade(possibleScore, studentScore) {
		const score = (studentScore / possibleScore) * 100;
		if(score >= 90) {
			return "A";
		} else if(score >= 80) {
			return "B";
		} else if(score >= 70) {
			return "C";
		} else if(score >= 60) {
			return "D";
		} else {
			return "F";
		}
	}
}

// Strategy
class RankGradeStrategy extends GradeStrategy {
	constructor() {
		super();
	}

	grade(possibleScore, studentScore, allScores) {
		const sortedScores = [...allScores].sort((a, b) => b - a);
		return sortedScores.indexOf(studentScore) + 1;
	}
}
~~~

(live version)[https://repl.it/@I3uckwheat/strat-pat-examp#index.js]

Notice how the "context" in this example is what gets passed a grading strategy. This is used as the messenger between your application and the strategies. Each strategy has the same interface, and the context can use that to pass the proper data to it. Using a context makes it very simple to modify the API of the strategies without having to change every class that whishes to use that strategy. It also helps give a uniform API for managing strategies. 

`GradeStrategy`, which is the abstract strategy, exists to create an interface that each concrete strategy implements. This could also be non-abstract and hold default behavior for all strategies that inherit from it. 

`PercentGradeStrategy`, `LetterGradeStrategy`, and `RankGradeStrategy` are all children of the `GradeStrategy` class and implement the same interface. Even though I didn't do it here, it's important to note that you can hold state within the strategies. An example of when that could be useful is AI in a game. Consider an NPC (non player character), which has a `Movement` context that calls methods on different `Move` strategies. One such strategy could be an "aggressive" movement strategy that prevents the character from ever entering a place it's been before. You could hold a list of places that entity has been within the strategy, as state. 

One more important topic to cover with this pattern is having the ability to switch out which pattern is used, during runtime. This is often accomplished with a method on the context class that can take in a strategy and assign that new strategy to the context's state. 

Contexts can also decide which strategy to use if the situation calls for it, a good example would be a sorter. Sometimes a quick-sort is better suited than a merge sort. The context can take in data, and analyze it before deciding which algorithm (strategy) to use with this data. This is more useful in strongly typed languages.


## Where is it used?

The Strategy Pattern is often used in things like game-engines, an example of such is a `Collider`, which has to take in a `Shape`. That `Shape` is one of any type of shape that the engine can handle (you can even write your own `Shape`). These shapes are strategies that the `Collider` can use to help with calculations. 

I also suspect SnapChat in using the strategy pattern. Consider each filter a `RenderStrategy` that the `Filter` context can use. 

## Example Case Study

Below is a link to a more fleshed-out example that can be played with and run, inspired by SnapChat.

[Live Example](https://repl.it/@I3uckwheat/strategic-printer#script.js)

A few important things to point out:

* Strategies can have their own private methods, just like any other class.
* Each strategy varies in complexity.
* The `FilterApplicator` has default functionality that the `NoFilterApplicator` utilizes.
* The abstract strategy class can hold some helpers that the sub-classes can utilize.


## Compared to the Template Method Pattern

The Strategy Pattern and [Template Method Pattern](/blog/template-method-pattern) can both be used to solve the same problems, but generally the strategy pattern is much more flexible and has more obvious use-cases. The biggest difference between these two patterns is being able to change parts of an algorithm (template method), or the entire algorithm itself (strategy). Because the Strategy Pattern is more composable, it's preferred in most cases.

## Conclusions

This pattern is one that many developers implement without having to be exposed to it formally. It's a natural way to compose classes, and has many use-cases. Most notably, to reduce `if` statements to decide how something should behave.

## More Resources

* [A short overview of this pattern](https://refactoring.guru/design-patterns/strategy)
* [Another short overview](https://sourcemaking.com/design_patterns/strategy)
* An example being used in the real world
  * [context](https://github.com/TheOdinProject/theodinproject/blob/master/app/services/discord_notifier.rb)
  * [a strategy](https://github.com/TheOdinProject/theodinproject/blob/master/app/services/notifications/flag_submission.rb)
  * [another strategy](https://github.com/TheOdinProject/theodinproject/blob/master/app/services/notifications/daily_summary.rb)
