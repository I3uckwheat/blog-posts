# Observer Pattern

The observer pattern is used when multiple pieces of the application need to operate on the same data, and need to know when it has updated so they can update their own state, or operate on the data. It's commonly used in different views of the same data. This has also been called the "publish subscribe" pattern, or "pubsub".

## What is it?

The Observer pattern is a behavioral design pattern to help organize data that needs to be accessed from multiple places. This is very similar to how events work, with one difference: the Observer pattern has an explicit connection between two objects where one reacts to the other updating, while events can be thrown into the void and allow whatever is listening to do what they want. 

This pattern is made up of two parts, the "subject" and "observers". The subject is a class that holds the single source of truth, a list of subscribed observers ready to react to the data, and a way to attach and detach an observer. When the subject's state changes, the `notify` method will then iterate over the subscribed objects and call the `update` method on each of them. The Observer will then request the data from the subject and then handle the new data as it sees fit. The new state can also be passed through the `update` method.

This may seem familiar if you have used the class based syntax with React.js

~~~javascript
class Subject {
	constructor() {
		this.observers = [];
		this.state = {};
	}

	attach(observer) {
		this.observers.push(observer);
	}

	detach(observer) {
		this.observers = this.observers.filter(observerA => observerA !== observerB);
	}

	notify() {
		this.observers.forEach(observer => observer.update(this.state));
	};

	setState(newState) {
		this.state = {...this.state, ...newState};
		this.notify();
	}
}

class Observer {
	constructor() {
	}

	update(changedSubject) {}
}


class ConcreteObserver extends Observer {
	constructor() {
		super()
	}

	update(state) {
		this.printState(state);
	}

	printState(state) {
		console.log(state)
	}
}

class ConcreteObserver2 extends Observer {
	constructor() {
		super()
	}

	update(state) {
		this.printState(`Current state observed is: ${JSON.stringify(state)}`);
	}

	printState(state) {
		console.log(state)
	}
}
~~~

In this example, the observers have no internal state, however an observer can hold their own state as they need and process the updated data however they wish. This extends beyond different ways of displaying data.

## Where is it used?

This pattern is used quite frequently in UI frameworks and patterns to help with updating specific pieces of the UI. This is something one would expect to see often in MVC patterns that aren't database based. 

## Example Case Study

Below is a link to a more fleshed-out example that can be played with and run.

[Live Example](https://repl.it/@I3uckwheat/bird-observer#script.js)

A few important things to point out:

* The subject can update state however it needs to, in this case methods are used.
* Observers can have their own state.
* Observers can handle the data however they need, they do not need to change how things are displayed, an Observer is just a class that is notified of a change from a subject.
* In a weakly typed language, like JavaScript, one doesn't need to inherit from an "observer" class, and one can just have the proper methods available. However, doing this does make it harder to identify something is an observer at times. 
    * However, clever use of JS allows mixins to be used instead: [see here](https://repl.it/@codyloyd/ObserverMixins#index.js)


## Conclusions

This is a pattern that seems to come up often whenever data needs to be acted upon in real-time, and a fairly common pattern. Granted it's a simple one to understand, being creative with it can lead to some very easy to maintain code.

## More Resources

* [A short overview of this pattern](https://www.dofactory.com/javascript/design-patterns/observer)
* [Another nice overview](https://www.tutorialspoint.com/design_pattern/observer_pattern.htm)
* [An example in game programming](https://gameprogrammingpatterns.com/observer.html)