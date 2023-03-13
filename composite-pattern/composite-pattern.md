# Composite Pattern

This is a very common pattern used in UI libraries, however, the actual implementation is often abstracted to us as developers. After understanding this pattern, you will start seeing places it exists and recognizing how software is written.

## What is it?

This pattern describes a tree-like structure with "composites" and "leaves". A composite is a grouping of leaves. This pattern is used to describe items where containers of items have the same interface or when you want to describe part-whole hierarchies of objects. The hard part of this pattern isn't the implementation, or theoretical understanding; but finding a use-case for it.

This pattern requires a few classes to implement: a Component, Leaf, and Composite. 

* A Component is the base-class that Leaf and Composite inherit from.
* A Leaf represents the end objects in the hierarchy, these will be attached to Composites
* A Composite has leafs or other composites to build out the hierarchy

There is much more nuance that goes on with this pattern however, some of those nuances are outlined below
* You can have static leafs that can be appended to the tree in multiple places
* Often one has to traverse the tree to get information from the parent. With a large tree this can get expensive, so caching is an important topic
* Leaves can be multiple types, one could draw a line, another could draw a rectangle

~~~javascript
// Not really needed in JS, but useful for strict typed languages. This is basically the interface the child classes will adhere to
class Component {
	constructor() {
	}

	doSomething() {}

	add(item) {
	}

	remove(toRemove) {
	}
}

class Composite extends Component {
	constructor() {
		super();
		this.children = [];
	}

	doSomething() {
		this.children.forEach(child => child.doSomething());
	}

	add(item) {
		this.children.push(item);
	}

	remove(toRemove) {
		this.children = this.children.filter(child => child !== toRemove);
	}
}

class Leaf extends Component {
	constructor(value) {
		this.value = value;
		super();
	}

	doSomething() {
		console.log(this.value)
	}
}

const root = new Composite();
root.add(new Leaf('foo'))
root.add(new Leaf('bar'))
root.add(new Leaf('baz'))

const branch = new Composite();
branch.add(new Leaf('hello'))
branch.add(new Leaf('world'))

root.add(branch);
root.doSomething();
~~~

In the example above, you can see how each `Composite` has the ability to add leaves and other composites to it. Each leaf only *does* something, but does not have the ability to add more leaves to itself. If you are creative, you can do interesting things with this pattern. 

## Where is it used?

The most prominent place this pattern is used is in the DOM. The DOM is a hierarchial system of "nodes" that do things. If you look back into old JS, you can find a "text node" which is a leaf that can be applied to a composite (`<div>`, `<p>`, `<a>`) to render text. This is clear evidence of the DOM using this pattern.

Other than the DOM, any sort of UI framework likely uses this pattern. React for example.

## Example Case Study

Below is a link to a more fleshed-out example that can be played with and run.

[Live Example](https://repl.it/@I3uckwheat/composite-bot)

A few important things to point out:

* The getCtx() and the getAbsolutePosition() both have to traverse the tree to get data, this is a prime place for caching. 
* There are two kinds of leaves here, an animated one and a non-animated one. Instead of having an animated leaf, one could have a composite that takes in leaves for animations. 
* If you move the position of any composite, all its children move as well.

## Conclusions

This is a pattern with many uses, with a VERY clear indication when it's needed. Though, it's often abstracted away from most developers with modern frameworks.

## More Resources

- [A more in depth example of how to use this pattern](https://jsmanifest.com/the-composite-pattern-in-javascript/)
