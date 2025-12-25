# React 

## Installation
You can use exsiting full stack frameworks like NextJS, or build it from 
scratch.

### Building from scratch
Step 1: Install a build tool
Install build like vite, parcel, or rsbuild

Note - Metro for React Native. It is a JS bundler for RN. Metro supports
bundling for platforms like iOS and Android.

Step 2: Building common application patterns
* Routing
* Data Fetching
* Code-splitting
* Improving application performance


## Managing State

### Chosing the state structure
Principles for structuring state:

1. **Group related state**: if you always update two or more state variables
	consider merging them into a single state.
	Another case where you'll group data into an object or an array is when
	you don't know how many pieces of state you'll need. For example, it's 
	helpful when you have a form where the user can add custom fields.
	Note - if your state variable is an object, remember that you can't update
	only one field in it without explicitly copying the other fields.

2. **Avoid contradictions in state**: When the state is structured in a way 
	that several pieces of state may contradict and disagree with each other,
	you leave room for mistakes. Try to avoid this.

3. **Avoid redundant state**: if you calculate some information from the 
	component's props or its existing state variables during rendering, you
	should not put that information into that component's state.

4. **Avoid duplication in state**: When the same data is duplicated between
	multiple state variables, or within nested objects, it is difficult to 
	keep them in sync. Reduce duplication when you can.
	Example: when selecting item from the list store only id instead of whole
	item object. With this it will not be out of sync.

5. **Avoid deeply nested state**: Deeply hierarchical state is not very 
	convenient to update. When possible, prefer to structure state in a flat
	way.
	It makes easier to update, and it helps ensure you don't have duplication
	in different parts of a nested object.
	Sometimes, you can also reduce state nesting by moving some of the nested
	state into the child components.This works well for ephemeral UI state 
	that doesn't need to be stored, like whether an item is hovered.

The goal behind these principles is to make state easy to update without 
introducing mistakes. *Make you state as simple as it can be - but simpler*

Note - All the normal variables are calculated during render.
Note - Don't mirror props in state. lets understand this throught example: 

`
	funtion Message({messageColor}) {
		const [color, setColor] = useState(messageColor);
	}
`

Here a color state variable is initialized to the messageColor prop. The 
problem is that if the present component passes a different value of 
messageColor later(for example, 'red' instead of 'blue'), the color sate 
variable would not be updated! The state is only initialized during the first
render. Instead, use prop directly in your code. This way it won't get out of
sync with the prop passed from the parent component.
"Mirrorin" props into state only make sense when you want to ignore all 
updates for a specific prop. By convention, start the prop name with initial
or default to clarify that it's new values are ignored.


### Sharing state between components
Sometimes, you want the state of two components to always change together. To
do it, remove state from both them, move it to their closest common parent, 
and then pass it down to them via props. This is known as lifting state up, 
and it's one of the most common things you will do writing React code.

Three steps to lift state up:
1. Remove state from the child components.
2. Pass hardcoded data from the common parent.
	To lift state up, you must locate the closest common parent component of
	both of the child components that you want to coordinate. The parent will
	become "source of truth" for children.

3. Add state to the common parent and pass it down together with the event
	handlers.
	The event handlers are passed down to children to allow them to change the
	state explictly.

Controlled and uncontrolled components:
It is common to call a component with some local state "uncontrolled". 
In contrast, you might say a component is "controlled" when the important 
information in it is driven by props rather than its own local state. This 
lets the parent component fully specify its behaviour. 

Uncontrollled components are easier to use withint their parents because they
required less configuration. But they're less flexible when you want to 
coordinate them together. Controlled components are maximally flexible, but 
they require the parent components to fully configure them with props.

In practice, "controlled" and "uncontrolled" aren't strict technical terms -
each component usually has some mix of bothe local and props. However, this is
useful way to talk about how components are designed and what capabilities 
they offer.

Whey writting a component, consider which information in it should be 
controlled (via props), and which information should be uncontrolled (via 
state). But you can always change your mind and refactore later.


-> In a React application, many components will have their own state. Some
state may "live" close to the leaf components( components at the bottom of the
tree) like inputs. Other state may "live" closer to the top of the app. For
expample, even client-side routing libs are usually implemented by storing
the current route in the React state, and passing it down by props.
For each unique piece of state, you will choose the component that "owns" it.
This principle is also known as having a "single source of truth". It doesn't
mean all state lives in one place - but that each piece of state, there is 
specific component that holds that peice of information. Instead of duplicate
shared state between components, lift if up to their common shared parent, and
pass it down to the children that need it.
As your app changes, you move state up and down according to the needs. This
is all part of the process!


### Preserving and Resetting State
State is isolated b/w components. React keeps track of which state belongs to
which component based on their place in the UI tree. You can control when to
preseve state and when to reset it b/w re-renders.

-> State is tied to a position in the render tree:
React builds render trees for the component structure in your UI.

When you give a component state, the state is actually held inside React. React
associates each piece of state it's holding with the correct component by where
that component sits in the render tree. 
Each component on the screen has fully isolated state. React will keep the state
around for as long as you render the same component at the same position in the
tree. When React removes a component, it destroys its state.
React preserves a component's state for as long as it's being rendered at its
position in the UI tree. If it gets removed,  or a different component gets
rendered at the same position, React discards its state.

-> Same component at the same position preserves state: see the Counter example
where isFancy prop is sent passed to it. Whether isFancy is true or false, you
always have Counter as the first child of the div returned from the root App
component.

Note - Remember that it's the position in the UI tree - not in the JSX markup -
that matters to React!

-> Different components at the same position reset state:
When you render a different component in the same position, it resets the state
of its entire subtree.

`
	 {isFancy ? (
	   <div>
			<Counter isFancy={true} /> 
		</div>
	 ) : (
		<section>
			<Counter isFancy={false} />
	   </section>
	   )}
`

In this state gets reset. Although you render a Counter, the first child of the
div changes from a section to a div. When the child section was removed from the
DOM, the whole tree below it was destryed as well.
AS a rule of thumb, if you want to preserve the state between re-renders the
structure of your tree needs to "match up" from one render to another. If the
structure is different, the state gets destroyed because React destroys state
when it removes a component from the tree.

Note - Always declare component functions at the top level, and don't nest their
definitions.

-> Resetting state at the same position:
Sometimes you may want to reset a component's state at the same position.
There are two ways to reset state when switching between them:
1. Render components in different positions
	This solution is convenient when you have few independent components 
	rendered in the same place.

2. Give each component an explicit identiy with key
	More generic way to reset a component's state. You can use keys to make 
	React distinguish b/w any components. By default, React uses order within
	the parent to discern b/w components. But keys let you tell React that this
	is a specific component.
	Switching b/w components does not preserve state. This is because you gave
	them different keys.
	Specifying a key, tells React to use the key itself as part of the position
	, instead of their order within the parent. This is why, even though you 
	render them in the same place in JSX, React sees them as two differnt 
	counter appears on the screen, its state is created. Every time it is 
	removed, its state is destroyed. Toggling b/w them resets their state over
	and over.

Note - Remember the keys are not globally unique. They only specify the position
within the parent.

-> Resetting a form with key:
Resetting state with a key is particularly useful when dealing with forms. 
Whey you pass key the component will be recreated from scracth, including any
state in the tree below it. React will also re-create the DOM elements instead
of resuing them.

Note - Preserving state for removed components:-
Three ways:
1. Render all components and hide them with css and conditionals. Problematic
for large and contain a lot of DOM nodes.

2. Lift the state up. Most common solution

3. You might use a different source in addition to React state. For example,
you probably want a message draft to persist even if the user accidentally 
closes the page. To implement this, you could have the Chat component initialize
its state by reading from the localStorage, and save the drafts there too.

Recap:
* React keeps state for as long as the same component is rendered at the same
position.
* State is not kept in JSX tags. It's associated with the tree position in 
which you put in that JSX.
* You can force a subtree to reset its safe by giving it a different key.
* Don't nest component definitions, or you'll reset state by accident.


### Extracting State Logic into a Reducer
Components with many state updates spread across many event handlers can get 
overwhelming. For these cases, you can consolidate all the state update logic 
outside your component in a single function, called a reducer.

-> Consolidate state logic with a reducer:
To reduce complexity and keep all your logic in one easy-to-access place, you 
can move the state logic into a single function outside your component, called a
"reducer".
Reducers are a different way to handle state. You can migrate from useState to
useReducer in three steps:

1. **Move** from setting state to dispatching actions.
	Managing state with reducers is slightly different from directly setting 
	state. Instead of telling React "what to do" by setting state, you specify
	"what the user just did" by dispatching "actions" from your event handlers.
	Example:

	`
		dispatch({
			type: 'added',
			id: nextId++,
			text: text
		})
	`

	The object you pass to dispatch is called an "action". It is regular JS 
	object. It should contain the minimal information about what happened. An
	action object can have any shape. By convention, it is common to give it a
	string type that describes what happened, and pass any additional info in 
	other fields.

2. **Write** a reducer function.
	A reducer funciotn is where you put your state logic. It takes two arguments
	, the current state and the action object, and it returns next state:

	`
		// use switch statement to track actions. just like redux

		function reducerFunc(state, action) {
			switch(action.type) {
				case 'added': {
					return [...state, { id: action.id, text: action.text }]
				}
				
				default: { throw Error('Unknown action: ' + action.type }
			}
		}

		// use braces so that variables declared inside of different cases don't
		clash with each other.
	`

	React will set the state to what you return from the reducer. Because the
	reducer function takes state as an argument, you can declare it outside of 
	your component. This decreases the indentation level and can make your code
	easier to read.

3. **Use** the reducer from your component.
	Import the useReducer Hook from React and replace the useState with it.

	`
		import { useReducer } from 'React';

		const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);
	`

	The useReducer Hook takes two arguments:
	* A reducer function
	* An initial state

	And it returns:
	* A stateful value
	* A dispatch function (to "dispatch user actions to the reducer)

	You can move the reducer to a different file. Component logic can be easier
	to read when you separate concerns like this. Now the event handlers only 
	specifiy what happened by dispatching actions, and the reducer function
	determines how the state updates in response to them.


-> Writing reducers well:
Two tips:

1. Reducers must be pure : like state updater functions, reducers run during
	rendering (actions are queued until the next render). This means they must
	be pure - same inputs always result in the same output. They should not send
	requests, schedule timeouts, or perform any side effects (operations that 
	impact things outside the component). They should update objects and arrays
	without mutations.

2. Each action describes a single user interaction, even if that leads to 
	multiple changes in the data.



### Passig Data Deeply with Context
Usually you will pass information from a parent component to a child component
via props. But if tree size is large and receiving component is down the tree
then prop drilling is not best way. Context lets the parent component make some
information available to any component in the tree below it - no matter how deep
- without passing it explicitly through props.

You add context in 3 steps:

1. **Create** a context.
2. **Use** the context from the component that needs the data.
3. **Provide** that context from the component that specifies thet data.

Context lets a parent - even a distant one - provide some data to the entire 
tree inside of it.

-> Step 1: Create the context: You need to create the context. You'll need to
export it from a file so that your components can use it. You can declare 
context in the same file as component, but it's better practice to separate the
concern.

`
	import { createContext } form 'react';

	export const LevelContext = createContext(1);
`

-> Step 2: Use the context: import the useContext Hook from React and your 
context:

`
	import { useContext } from 'react';
	import { LevelContext } from './LevelContext.js';

	export default function Heading({children}) {
		const level = useContext(LevelContext);
		//...
	}
`

useContext is a Hook. It tells React that the Heading component wants to read
the LevelContext in the given example.

-> Step 3: Provide the context:

`
	import { LevelContext } from './LevelContext.js';
	export default function Section({level, children}) {
		return (
			<section>
				<LevelContext value={level}>
					{children}
				</LevelCotext>
			</section>
		)
	}
	// This tells React: "if any component inside this <Section> asks for Level
	// Context, give them this level. The component will use the value of 
	// nearest <LevelContext> in the UI tree above it.
`

Note - Context passes through intermediate components. Context lets you write
components that 'adapt to their surroundings' and display themselves differently
depending on where(or, in other words, in which context) they are being rendered
. How context works might remind you of CSS property inheritance. Different 
React contexts don't override each other. Each context that you make with 
createContext() is completely separate from other ones, and ties together 
components using and providing that particular context. One component may use or
provide many different contexts without a problem.

-> Few alternatives before you use context:

1. Start by passing props.
2. Extract components and pass JSX as children to them.

-> Use cases for context

* Theming
* Current accout
* Routing
* Managing state

In general if some information is needed by distant components in differnt parts
of the tree, it's a good indication that context will help you.


### Scaling up with Renducer and Context
Reducers let you consolidate a component's state update logic. Context lets you
pass information deep down to other components. You can combine reducers and 
context together to manage state of a complex screen.

Following are the steps to combine a reducer with context:

1. **Create** the context
2. **Put** state and dispatch into context
3. **Use** context anywhere in the tree

Note - You will create two separate contexts. One for providing the actual state.
Second for the providing the functions that lets the components dispatch actions.
Export them from a separate file so that you can later import them from other 
files. see docs of how to?

Note - You import context in you component. Take state and dispatch returned by
useReducer() and provide them to the entire tree below. Now you don't need to 
pass the state and or the event handler down the tree. Instead use useContext() 
to read both state and dispatch from any component below the tree. The state 
still 'lives' in the top-level component, managed with useReducer.

-> Moving all wiring into a single file:- see docs for how to do. You can create
a custom hook to return context values, and funcs.

## Escape Hatches

### Referencing Values with Refs
When you want a component to remember some info but without triggering new
renders, you can use ref.

-> Adding a ref to your component

`
	import { useRef } from 'react';
	// inside the component

	const ref = useRef(0);
`

Inside your component, call the useRef Hook and pass the initial value that you
want to reference as the only argument. For example see above.

useRef returns an object like this:

`
	{
		current: 0	// The value you passed to useRef
	}
`

You can access the current value of the ref through the `ref.current` property.
This value is intentionally mutable, meaning you can both read and write to it.
It's like a secret pocket of your component that React doesn't track. (This is 
what makes it an 'escape hatch' from React's one-way data flow)
The ref, like state, could point to anything: a string, an object, a number, or
even a function. Unlike state, ref is plain JS object with the current property
that you can read and modify.

Note - Like state, refs are retained by React b/w re-renders. However, setting 
state re-renders a component. Changing a ref doesn't. You can combine refs and
state in a single component.

When a piece of information is used for rendering, keep it in state. When a piece
of information is only needed by event handlers and changing it doesn't require
a re-render, using a ref may be more efficient. When something is displayed on 
the screen it makes sense to use state value for it. Setting a state triggers the
re-render that reflect the new value. If you try to do this with refs React would
never re-render the component and latest update will not be reflected on the 
screen. 
This is why reading ref.current during render leads to unreliable code. if you 
need that, use state instead.

-> When to use refs: use a ref when your component needs to 'step outside' React
and communicate with external APIs - often browser API that won't impact the 
appearance of the component.

* Storing timeout IDs
* Storing and manipulating DOM elements
* Storing other objects that are aren't necessary to calculate the JSX.

If your component needs to store some value, but it doesn't impact the rendering
logic, choose refs.

-> Best practices for refs:

* Treat refs as an escape hatch.
* Don't read or write ref.current during rendering: if some information is needed
during rendering, use state instead. Since React doesn't know when ref.current
changes, even reading it while rendering makes your component's behaviour hard to
prodict. (The only exception to this is code like ` if(!ref.current) ref.current 
= new Thing()` which only sets the ref once during the first render.

Limitations of React state don't apply to refs. For exmaple, state acts like a
snapshot of for every render and doesn't update syncronoulsy. But when you mutate
the current value of a ref, it changes immediately. This is because the ref 
itself is a regular JS object, and so it behaves like so. You can mutate it as 
long as mutating isn't used for rendering.

### Manipulating the DOM with refs
Sometimes you might need access to the DOM elements managed by React - for 
example, to focus a node, scroll to it, or measure its size and position. You 
will need a ref to the DOM node.

-> Getting a ref to the node

`
	import { useRef } from 'react';

	// inside the component
	const myRef = useRef(null);
	<div ref={myRef} >
`

The useRef Hook returns an object with a single property called current.
Initially, myRef.current will be null. When React creates a DOM node for this
<div>, React will put a reference to this node into myRef.current. You can then
access this DOM node from your event handlers and use the built-in browser APIs
defined on it.

Sometimes you might need a ref to each item in the list, and you don't know how
many you will have. One solution is to pass a function to the ref attribute. This
is called a ref callback. React will call your ref callback with the DOM node 
when it's time to set the ref, and call the cleanup function returned from the
callback when it's time to clear it. This lets you maintain your own array or a 
Map, and access any ref by its index or some kind of ID. see docs.

Note - Refs are escape hatch. Manually manipulating another component's DOM nodes
can make your code fragile.

-> Accessing another component's DOM nodes: You can pass refs from parent 
component to child components just like any other prop.
When ref is passed to another component as prop, you may want to restrict the 
exposed functionality. You can do that with useImeprativeHandle

`
	function Myinput({ref}) {
		const realInputRef = useRef(null);
		useImperativeHandle(ref, () => ({
			focus() {
				realInputRef.current.focus();
			}
		}));
		return <input ref={realInputRef} />
	}
`
Here, realInputRef inside MyInput holds the actual input DOM node. However, 
useImperativeHandle instructs React to provide your own special object as the 
value of a ref to the parent component. So inputRef.current inside the Form 
component will only have the focus method. In this case, the ref "handle" is not
the DOM node, but the custom object you create inside useImperativeHandle call.

-> When React attaches the refs:
In React, every update is split in two phases:

* During **render**, React calls your components to figure out what should be on
the screen.
* During **commit**, React applies changes to the DOM.

In general, youi don't want to access refs during rendering. That goes for refs 
holding DOM nodes as well. During the first render, the DOM nodes have not yet
been created, so ref.current will be null. And during the rendering of updates,
the DOM nodes haven't been updated yet. So it's too early to read them.

React sets ref.current during the commit. Before updating the DOM, React sets the
affected ref.current values to null. After updating the DOM React immediately 
sets them to the corresponding DOM nodes.

Usually, you will access refs from event handlers. If you want to do something
with a ref, but there is no particular event to do it, you might need an Effect.

-> Flushing state updates asynchronously with flushSync
Imagine there is list of todos and everytime you add todo, with the help of ref,
you try to display last element using scrollIntoView. But it won't happen and 
only shows the second last todo.
In React, state updates are queued. However, here it causes a problem because
setTodos does not immediately update the DOM. So the time you scroll the list to
its last element, the todo has not yet been added. This is why scrolling always
"lags behind" by one item.
To fix this issue, you can force React to update ("flush") the DOM synchronously.
To do this, import *flushSync* from react-dom and wrap the state update into a 
flushSync call:

`
	flushSync(() => {
		setTodos([...todos, newTodo]);
	});
	listRef.current.lastchild.scrollIntoView();
`

This will instruct React to update the DOM synchronously right after the code
wrapped in flushSync executes. As a result, the last todo will already be in the
DOM by the time you try to scroll to it.

-> Best practices for DOM manipulation with refs:
Refs are an escape hatches. You should only use them when you have to "step 
outside React". Common examples of this include managing focus, scroll position,
or calling browser APIs that React does not expose.
If you stick to non-destructive actions like focusing and scrolling, you 
shouldn't encounter any problems. However, if you try to modity the DOM manually,
you can risk conflicting with the changes React is making.	

* **Avoid changin DOM nodes managed by React**. Modifying, adding children to, or
removing children from elements that are managed by React can lead to 
incosistent visaul results or crashes like above.

* **You can safely modify parts of the DOM that React has no reason to update**.


### Synchronizing with Effects
Effects let you run some code after rendering so that you can synchronize your
component with some system outside of React.

-> What are Effects and how are they different from events?
There are two types of logic inside React components:

1. Rendering code :- Lives at the top level of your component. This is where you
take the props and state, transform them, and return the JSX you want to see on 
the screen. Rendering code must be pure like a math formula.

2. Event handlers (introduced in Adding interactivity):- they are nested funcs 
inside your components that do things rather than just calculate them. An event
handler might update an input field, submit an HTTP post request a buy product,
or navigate the user to another screen. Event handlers cantain "side effects"
caused by a specific user action.

Sometimes this isn't enough. Consider a ChatRoom component that must connect to
the chat server whatever it's visible on the screen. Connecting to a server is 
not a pure calculation (it's a side effect) so it can't happen during rendering.
However, there is no single particular event lika a click that causes ChatRoom
to be displayed.

**Effects let you specify side effects that are caused by rendering itself, 
rather than by a particular events**. Sending a message in the chat is an event
because it is directly caused by the user clicking a specific button. However,
setting up a server connection is an Effect because it should happen no matter
which interaction caused the component to appear. Effects run at the end of a
commit after the commit after the screen updates. This is a good time to sync
the React components with some external system.

-> You might not need an effect:- Don't rush to add effects to your components.
Keep in mind that Effects are typically used to "step out" of your React code
and sync with some external system. This includes browser APIs, third-party 
widgets, network, and so on. If your effect only adjusts some state based on 
other state, you might not need an Effect.

-> How to write an Effect:-
Steps to write an Effect:

1. Declare an Effect:- By default, you Effect will run after every commit.
	Everytime your component renders, React will update the screen and then run
	the code inside useEffect. In other words, useEffect "delays" a piece of 
	code from running until that render is reflected on the screen.

	Note - Don't try to do something with DOM node during rendering. In React,
	rendering should be a pure calculations of JSX and should not contain side
	effects like modifying the DOM. Moreover during first rendering the DOM 
	doesn't exist yet; the app will break. So wrap the side effect with 
	useEffect to move it out of the rendering calculation. By doing this, you
	let the React update the screen first. Then your Effect runs.
	
	Note - Effects run after every render.

2. Specify the Effect dependencies:- Mosts effects should only re-run when 
needed rather than after every render.
	You can tell React to skip unnecessarily re-running the Effect by specifying
	an array of dependencies as the second argument to the useEffect call. Start
	by adding an empty [] array as the second argument. An empty array makes the
	useState runs only once after the render and prevents re-running on every 
	re-render.
	The dependency array can contain multiple dependencies. React will only skip
	re-running the Effect if all of the dependencies you specify have exactly the
	same values as they had during the previous render. React compares the 
	dependency values using the Object.is comparision. 

	Note - The ref object has a stable identity: React guarantees you'll always
	get the same object from the same useRef call on every render. It never 
	changes, so it will never by itself cause the Effect to re-run. Therefore, it
	does not matter whether you include it or not. Including it is fine too.
	The set function returned by useState also have stable identity, so you'll
	often see them omitted from the dependencies too. If the linter lets you omit
	a dependency without errors, it is safe to do.
	Omitting always-stable dependencies only works when the linter can "see" that
	the object is stable. For exmaple, if ref was passed from a parent component,
	you would have to specify it in the dependency array. However, this is good
	because you can't know whether the parent component always passes the same ref
	, or passes one of several refs conditionally. So you Effect would depend on 
	which ref is passed.

3. Add cleanup if needed:- Some Effects need to specify how to stop, undo, or
clean up whatever they were doing.
	The code inside the Effect doesn't use any props or state, so your dependency
	array is []. This tells React to only run this code when the component 
	"mounts", i.e. appears on the screen for the first time.
	Suppose a component makes a connection outside and then you navigate to other
	page. The previous component is unmounted but the connection is not destroyed.
	When you come back that component again then connection is again established.
	This way is connections would keep piling up.
	To help you spot them quickly, in development React remounts every component
	once immediately after its inintal mount.
	To fix this issue, return a cleanup function from your Effect. React will call
	you cleanup function each time before the Effect runs again, and one final 
	time when the component unmounts (gets removed).


-> How to handle the Effect firing twice in development?
React intentionally remounts your components in development to find bugs. The 
right question isn't "how to run an Effect once", but "how to fix my Effect so 
that it works after remounting".
Usually, the answer is to implement the cleanup function. The cleanup function
should stop or undo whatever the Effect was doing. The rule of thumb is that user
shouldn't be able to distinguish between the Effect running once (as in prod) and
a setup -> cleanup -> setup sequence (as you'd see in development).

Note - Don't use refs to prevent Effects from firing in development.

Most of the Effects you'll write will fit into one of the common patterns below:

* Controlling non-React widgets:
	Sometimes you need to add UI widgets that aren't written in React. For example

	`
		useEffect(() => {
			const map = mapRef.current;
			map.setZoomLevel(zoomLevel);
		}, [zoomLevel]);
	`

	Note that there is no cleanup is this case.

	Some APIs may not allow you to call them twice in a row. For example, the 
	showModal method of the built-in <dialog> element throws if you call it twice.
	Implement the cleanup function and make it close the dialog:

	`
		useEffect(() => {
			const dialog = dialogRef.current;
			dialog.showModal();
			
			return () => dialog.close();
		}, []);
	`

* Subscribing to events

	`
		useEffect(() => {
			function handleScroll(e) {
				console.log("window.scrollX, window.scrollY);
			}
			window.addEventListener('scroll', handleScroll);

			return () => window.removeEventListener('scroll', handleScroll);
		}, []);
	`

	In development, your Effect will call addEventListener(), then immediately
	removeListener(), and then addEventListener() again with the same handler.
	So there would be only one active subscription at a time. This has the same
	user-visible behavior as calling addEventListener() once, as in production.

* Triggering animations
	if your Effect animats something in, the cleanup function should reset the 
	animation to the initial values:

	`
		useEffect(() => {
			const node = ref.current;
			node.style.opacity = 1; // trigger the animation

			return () => {
				node.style.opacity = 0;
			}
		}, []);
	`

* Fetching data
	If your Effects fetches something, the cleanup function should either abort
	the fetch or ignore its result:

	`
		useEffect(() => {
			let ignore = false;

			async function startFetching() {
				const json = await fetchTodos(useId);
				if (!ignore) {
					setTodos(json);
				}
			}

			startFetching();

			return () => {
				ignore = true;
			}

		}, [userId]);
	`

	You can't undo a network request that already happened, but your cleanup 
	function should ensure that the fetch that's not relevant anymore doesn't keep
	affecting your application.

	Note - you can deduplicates requests and caches their response b/w components.
	This will not only improve the development experience, but also make your app
	feel faster. If you are using a framework, use it's built-in data fetching 
	mechanism. Otherwise, consider using or building a client-side cache: useSwR
	is a popular solution.

* Sending analytics:
	
	`
		useEffect(() => {
			logVisit(url);
		}, [url]);
	`

	see docs.

* Not an Effect: Initializing the application
	Some logic should only run once when the application starts. You can put
	outside your components. 

	`
		if (typeof window !== 'undefined') {
			// Check if we're running in the browser.
			checkAuthToken();
			loadDataFromLocalStorage();
		}

		function App() {
			...
		}
	`

	This guarantees that such logic only runs once after the browser laods the
	page.

* Not an Effect: Buying a product
	Such API calls must be placed in a button event handler


### You might not need an Effect
To help you gain the right intuition, let's look at some common concrete examples:

-> Updateing state based on props or state.
When something can be calculated from the existing props or state, don't put it in
state. Instead calculate it during rendering. This makes your code faster (you 
avoid the extra 'cascading" updates), simpler (you remove some code), and less
error-prone (you avoid bugs caused by different state variables getting out of sync
with each other). see Thinking in React.

-> Caching expensive calculations
You can cache an expensive calculation by wrapping it in a useMemo Hook.
Note - React compiler can automatically memoize expensive calculations for you,
eliminating the need for manual useMemo in many cases.

`
	cons visibleTodos = useMemo(() => {
		// doesn't re-run unless todos or filter change.
		return getFilteredTodos(todos, filter);
	}, [todos, filter]);
`

This tells React that you don't want the inner function to re-run unless either 
todos or filter have changed. 
The function you wrap in useMemo runs during rendering, so this only works for pure
calculations.

How to tell if a calculation is expensive?
In general unless you're creating or looping over thousands of objects, it's 
probably not expensive. You can console log to measure the time spent in a piece
of code.

`
	console.time('filter array');
	const visibleTodos = getFilteredTods(todos, filter);
	console.log("filter array");
`

useMemo won't make the first render faster. It only helps you skip unnecessary work
on updates.
Keep in mind that your machine is probably faster than your user's so it's a good
idea to test the performace with an artificial slowdown. For example, Chrome offers
a CPU throttling option for this.

Note - React compiler can autmatically memoize expensive calculations for you, 
eliminating the need for manula useMemo in many cases.

-> Resetting all state when a prop changes:
Normally, React preserves the state when the same component is rendered in the same 
sport, By pasing a key to the component, you're asking React to treat two instances of
a components with different key as two different components that should not share any
state. When the key (which you've set to) changes, React will recreate the DOM and
reset teh state of the profile component and all its children and all of its children.

-> Adjusting some state when a prop changes:
Sometimes, you might want to reset or adjust a part of the state on a prop change, but
not all of it.
Don't update the state in effect. Instead do it during rendering.
When you update a component during rendering, React throws away the returned JSX and 
immidiately retries rendering. To avoid very slow cascading retries, React only lets 
you update the same component's state during a render. If you update another component'
s state during a render, you'll see an error. A condition like items !== prevItems is
necessary to avoid loops. You may adjust state like this, but any other side effects
(like changing the DOM or setting timeouts) should stay in event handlers or Effects to
keep components pure.

`

	function List({ items }) {
		const [isReverse, setIsReverse] = useState(false);
		const [selection, setSelection] = useState(null);

		const [prevItems, setPrevItems] = useState(items);
		if (item !== prevItems) {
			setPrevItems(items);
			setSelection(null);
		}
		
		//...
	}

	function	

`

Although this pattern is more efficient than an Effect, most components shouldn't need
it either. No matter how you do it, adjusting state based on props or other state makes
you data flow more difficult to understand and debug. Always check whether you can 
reset all state with a key or calculate everything during rendering instead. e.g.
instead of storing (and resetting) the selected item, you can store the selected item 
ID.

`
	
	function List({item}) {
		const [isReverse, setIsReverser] = useState(false);
		const [selectedId, setSelectedId] = useState(null);
	
		// Best: calculate everything during rendering
		const selection = items.find(item => item.id === selectedId) ?? null;

	}

`

Now there is no need to 'adjust' the state at all. If the item with the selected ID is
in the list, it remains selected. If it's not, the seletion calculated during rendering
will be null because no matching item was found. This behaviour is different, but 
arguably better because most changes to items preserve the selection.

-> Sharing Logic b/w event handlers:
When you're not sure whether some code should be in a Effect or in an event handler, 
ask yourself why this code needs to run. Use Effects only for code that should run 
because the component was displayed to the user.

-> Sending a POST request:
Avoid: Event-specific logic inside an Effect
If you want to do something, set a state or send network request, at one specific 
moment in time - when the user presses the button - It should only ever happen on that
particular interaction - hence envent handler should handle those things.

When you choose whether to put some logic into an event handler or an Effect, the main
question you need to answer is what kind of logic it is from the user'e prespective. If
this logic is caused by a particular interaction, keep it in the event handler. If it's
caused by the user seeing the component on the screen, keep it in the Effect.

-> Chain of computations:
Sometimes you might feel tempted to chain Effects that each adjust a piece of state
based on other state.
There are two problems with this code:

1. The first problem is that it is very inefficient: the component (and its children)
have to re-render b/w each set call in the chain.

2. The second problem is that even if it weren't slow, as your code evolves, you will 
run unto cases where the 'chain' you wrote doesn't fit the new requirements.

In this case, it's better to calculate what you can during rendering, and ajust the 
state in the event handler. This is a lot more efficient. Now you will able to set 
each state variable to a move from the past without triggering the Effect chain that
adjusts every other value. If you need to reuse logic b/w several event handlers, you
can extract function adn call it from those handlers.

Remember that inside event handlers, stat behaves like a snapshot.

In some cases, you can't calculate the next state directly in the event handler. For
example, imagine a form with multiple dropdowns where the options of the next dropdown
depend on the selected value of teh previous dropdown. Then a chain of Effects is 
appropiate because you are synchronizing with network.

-> Initializing the application
Some logic should only run once when the app loads. You might be tempeted to place it 
in an Effect in the top-level component. However, you'll quickly discover that it runs
twice in development. This can cause issues - for example, maybe it invalidates the
authentication token because the function wasn't designed to be called twice. In 
general, your component should be resilient to be remounted. This includes your top-
level App component.
Although it may not ever get remounted in practice in production, following the same
constraints in all components makes it easier to move and reuse code. If some logic 
must run once per app load rather than once per component mount, add a top-level 
variable to track whether it has already executed.

`
	
	let didInit = false;

	function App() {
		useEffect(() => {
			if (!didInit) {
				// Only runs once per app load
			}
		}, []);

		// ...
	}

`

You can also run it during module initialization and before the app renders.

`

	if (typeof window !== 'undefined') {
		// Only runs per app load
	}

	function App() {
		// ...
	}

`

Code at the top level runs once when your component is imported - even it it doesn't
end up being rendered. To avoid slowdown or surprising behavior when importing
arbitrary components, don't overuse this pattern. Kee app-wide initialization logic to
root component madules lek App.js or in your application's entry point.


-> Notifying parent compoenents about state changes: see docs
Lift up the state instead.

-> Passing data to the parent
In React, data flows from the parent to their children. When you see something wrong on
the screen, you can trace where the information comes from by going up the component
chain until you find which component passes the wrong prop or has the wrong state. When
child components update the state of their parent components in Effects, the data flow
becomes very difficult to trace. Since both the child and the parent need the same data
, let the parent component fetch teh data, and pass it down the child instead.

This is simpler and keeps the data flow predictable: the data flow down from the parent
to the child.

-> Subscribing to an external store
Sometime, your component may need to subscribe to some data outside of the React state.
Since this data can change without React's knowledge, you may need to manually
subscribe your components to it. This is often done with an Effect.
Although it's common to use Effects for this, React has a purpose-built Hook for 
subscribiing to an external store that is prefferred instead. Delete the Effect and
replace it with a call to useSyncExternalStore.

`

	function subscribe(callback) {
		window.addEventListner('online', callback);
		window.addEventListner('offline', callback);

		return () => {
			window.removeEventListner('onlin', callback);
			window.removeEventListner('offline', callback);
		}
	}


	function useOnlineStatus() {
		return useSyncExternalStore( 
			subscribe, // React won't resubscribe for as long as you pass the same 
						// function
			() => navigator.onLine, // how to get the value on the client
			() => true  // how to get the value on the server
		);
	}


	function ChatIndicator() {
		const isOnline = useOnlineStatus();
		//...
	}
	
`

This approach is less error-prone than manually sncing mutable data to React state with
an Effect. Typically, you'll write a custom Hook like useOnlineStatus() above so that
you don't need to repeat this code in the individual components.

-> Fetching data
Avoid fetching without cleanup logic in an Effect.
The 'race condition': Two different requests 'raced' against each other and came in a 
different order that you expected.
To fix the race condition, you need to add a cleanup function to ignore stale responses

see example in the docs

Handling race conditions is not the only difficulty with implementing data fetching. 
You might also want to think about caching responses (so that the user can click Back 
and see the previous screen instantlyh), how to fetch data on the server (so that the
intial server rendered HTML constains the fetched content instead of a spinner), and 
how to avoid network waterfalls (so that a child can fetch data without waiting for 
every parent). Use frameworks for these solution.

If you don't use a framework, and don't want to build your own, but would like to make
data fetching from Effects more ergonomic, consider extracting your fetching logic into
a custom Hook. See docs for example. 
[data fetching](https://react.dev/learn/you-might-not-need-an-effect#fetching-data)

You'll likely also want to add some logic for error handling and to track whether the
content is loading. You can build a Hook like this yourself or use one of the many 
solutions already available in the React ecosystem.

In general, whenever you have to resort to writing Effects, keep an eye out for when you
can extract a piece of functionality into a custom Hook with a more declarative and 
purpose-built API. The fewer raw useEffect calls you have in your components, the easier
you will find to maintain your application.


## Lifecycle of Reactive Effects
Effects have a different lifecycle from components. Componnets may mount, update, or 
unmount. An Effect can only do things: to start synchronizing something, and later to 
stop synchronizing it. This cycle can happen multiple times if your Effect depends on 
props and state that change over time. React provides a  linter rule to check that you've
specified your Effects deps correctly. This your Effect sychronized to latest props and
state.

-> The lifecycle of an Effect
Every React component goes through the same lifecycle:

* A component mounts when it's added to the screen.
* A component updates when it receives new props or state, usually in response to an 
  interaction.
* A component unmounts when it's removed from the screen.

It's a good way to think about components, but not about Effects. Instead, try to think
about each Effect independently from your component's lifecycle. An Effects describes 
how to synchronize an external system to the current props and state. As your code 
changes, sychronization will need to happen more or less often.

see chatroom component example on the section's webpage

Intuitively, one might think React would start synchronizing and stop synchronizing upon
mount and unmount, but this is not the end of the story! Sometimes it may also be 
necessary to start and stop syncing multiple times while component remains unmounted.

Note - Some Effects don't return a cleanup function at all. More often than not, you'll
want to return one - but if you don't, React will behave as if you returned an empty
cleanup function.

-> How React re-synchroninzes you Effect:
To stop synchronizing, React will call the cleanup function that your Effect returned in
the previous render. Then React will run the Effect that you've provided during this
render.
Finally, when the user goes to a different screen - the component unmounts. Now there is
no need to stay connected at all. React will stop synchronizing your Effect one last time

-> Thinking from the Effect's perspective:
When you look from the component's perspective, it was tempting to think of Effects as 
"callbacks" or "lifecycle events" that fire at a specific time like "after a render" or
"before unmount". This way of thinking gets complicated very fast, so it's best to avoid.

Instead, always focus on single start/stop cycle at a time. It shouldn't matter whether
a component is mounting, updating, or unmounting. All you need to do is to describe how
to start synchronization and how to stop it. If you do it well, your Effect will be 
resilient to being started and stopped as many times as it's needed.

-> How React verifies that our Effect can re-synchronize:
Every time after your component re-renders, React will look at the array of dependencies
that you have passed. If any of the values in the array is different from the value at 
the same spot that you passed during previous render,React will re-synchronize your 
Effect. The different values are compared with Object.is.

-> Each Effect represents a separate syncrhonization process:
Resist adding unrelated logic to your Effect only because this logic needs to run at the
same time as an Effect you already wrote.
Each Effect in your code should represent a separate and independent synchronization
process.

-> Effects "react" to reactive values:
Props, state, and other values declared inside the component are reactive because they're
calculated during rendering and participate in the React data flow.

-> What an Effect with empty dependencies means:
Thinking from the component's perspective, the empty [] dependency array means this 
Effect executes the logic inside it when the component mounts, and perform any cleaning
logic only when the component unmounts. (Keep in mind that React would still 
re-synchronizing it an extra time in development to stress-test your logic).
However, if you think from the Effect's perspective, you don't need to think about 
mounting and unmounting at all. What's important is you've specified what your Effect
does to start and stop synchronizing. Today, it has no reactive dependencies. But if you
ever want to add reactive values to dependencies just add them in the dependency array 
and adjust the logic incorporating reactive values.

-> All variables declared in the component body are reactive:
Props and state aren't the only reactive values. Values that you calculate from them are
also reactive. If the props or state change, your component will re-render, and the 
values calculated from them will also change. This is why all variables from the 
component body used by the Effect should be in the Effect dependency list.
A regulare variable that you calculate during rendering is reactive, because it's 
calculated during rendering, so it can change due to a re-render.

All values inside the component (including props, state, and variables in your component'
s body) are reactive. Any reactive value can change on a re-render, so you need to 
include reactive values as Effect's dependencies.

In other words, Effects "react" to all values from the component body.

Note: Mutable values (inlcuding global variables) aren't reactive.
1. A mutable value like location.pathname can't be dependency. It's mutable, so it can
change at any time completely outside of the React rendering data flow. Changing it 
wouldn't trigger a re-render of your component. Therefore, even if you specified it in 
the dependencies, React wouldn't konw to re-synchronize the Effect when it changes. This
also breaks the rules of React because reading mutable data during rendering (which is 
when you calculate the dependencies) breaks purity of rendering. Instead,  You should 
read and sub to an external mutable value with useSyncExternalStore.

2. A mutable value like ref.current or things you read from it also can't be a dependency
. The ref object returned by useRef itself can be a dependency, but its current property
is intentionally mutable. It lets you keep track of something without triggering a 
re-render. But Since changin it doesn't trigger a re-render, it's not a reactive value,
and React won't konw to re-run your Effect when it changes.

-> React verifies that you specified every reactive value as dependency:
If your linter is configured for React, it will check and notify if any dependency is 
missing.

Note - In some cases, React knows that a value never changes even though it's declared
inside the component. For example, the set function returned from useState and the ref
object returned by useRef are stable - they are guaranteed to not change on a re-render.
Stable values aren't reactive, so you may omit them from the list. Including them is
allowed: they won't change, so it doesn't matter.

-> What to do when you don't want to re-synchronize:
You can prove to the linter that these values aren't reactive values, i.e. that they 
can't change as a result of a re-render. For example you can move them outside the
component; now they don't need to be dependencies.
You can also move them inside the Effect. They aren't calculated during rendering, so 
they're not reactive.

Effects are reactive blocks of code. They re-synchronize when the values you read inside
of them change. Unlike event handlers, which only run once per interationm, Effects run
whenever synchronization is necessary.

You can't choose your dependencies. Your dependencies must include every reactive value
you read in the Effect. The linter enforces this. Sometimes this may lead to problems 
like infinite loops and to your Effect re-synchronizing too often. Don't fix these 
problems by suppressing the linter! Here's what to try instead:

* Check that your Effect represents an independent synchronization process. If your
Effect doesn't sync anything, it might unnecessary. If it syncrhonizes several 
independent things, split it up.

* If you wnat to read the lates values of props or state without "reacting" to it and
re-synchronizing the Effect, you can split your Effect into a reactive part(which you'll
keep in the Effect) and a non-reactive part(which you'll extract into something called an
Effect event).

* Avoid relying on objects and functions as dependencies. If you create objects and 
functions during rendering and then read them from an Effect, they will be different on
every render. This will cause you Effect to re-synchronize every time.


## Separating Events from Effects
Event handlers only re-run when you perform the same interaction again. Unlike event
handlers, Effects re-synchronize if some value they read, like a prop or a state variable
, is different from what it was during the last render. Sometimes, you also want a mix
of both behaviors: an Effects that re-runs in response to some values but not others.

Should you use event handlers or Effects? Every time you need to answer this question, 
consider why the code needs to run.

* Event handlers run in response to specific interactions
* Effects run whenever synchronization is needed

-> Reactive values and reactive logic:
Props, state, and variables declared inside your component's body are called reactive
values. They participate in the rendering data flow.

Reactive values like these can change due to a re-render. Event handlers and Effects
respond to changes differently:

* Logic inside event handler is not reactive:- It will not run again unless the user 
performs the same interation (e.g. a click) again. Event handlers can read reactive 
values without "reacting" to their changes.

* Logic inside Effet is reactive:- If your Effect reads a reactive value, you have to 
specify it as a dependency. Then, if a re-render causes that value to change, React will
re-run your Effect's logic with the new value.
	

# Hooks list

1. useState

2. useContext

3. useReducer



