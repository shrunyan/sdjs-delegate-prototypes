# Delegate Prototypes

## Slide 1

__What are we going to talk about?__

- Creating a delegate prototype
- Using a factory to generate instances
- Use our object without the "new" keyword construction of an instance


## Slide 2

__What is a delegate prototype?__

First things first, we will create an object that will be the `prototype` for our other objects.

`Store.js`

	let Store = {
		date: {},
		get: (key) => {
			return this.data[key]
		},
		set: (key, el) => {
			this.data[key] = el
			return this.data[key]
		}
	}

	export default Store

* A Delegate prototype does just that, delegates. If a function can't be found at the current object it goes up the prototype chain to look for it.
* Bonus points: use an observable as your data prop


## Slide 3

__How do I use my delegate prototype?__

We'll create a factory which returns a new instance of our `Store` delegate extended by

`TimeStore.js`

	import Store from './Store'

	function TimeStore () {
		return Object.assign(Object.create(Store), {
			exists: (key) => {
				return this.data[key] ? false : true
			},
			getEpoch: () => {},
			getUTC: () => {}
		})
	}

	export default TimeStore()


* Object.assign: This is the same as `_.extend()` or `$.extend()`
* It is a new ES2015 feature and must be [polyfilled][1]
* Object.create: ES5 // TODO explain this
* This is our factory. // TODO explain a factory


## Slide 4

__How do I use this?__

`app.js`

	import TimeStore from './TimeStore'

	TimeStore.exists('now') // false
	TimeStore.set('now', Date()) // timestamp
	TimeStore.get('now') // timestamp
	TimeStore.exists('now') // true

* Show prototype chain in devtools
* Factory allowed use of object with out `new` keyword


## Slide 5

__So what... Why do I care?__

* decoupling
* SOLID???
* Avoid ES2015 `class`
* [OLOO][2]


## Slide 6

@stuartrunyan

References:

- Eric Elliot. [Misconceptiosn about inheritance][3]
- @getify. [You Don't Know JavaScript][4]



[1]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign#Polyfill
[2]: https://gist.github.com/getify/5572383
[3]: https://medium.com/javascript-scene/common-misconceptions-about-inheritance-in-javascript-d5d9bab29b0a
[4]: https://github.com/getify/You-Dont-Know-JS
