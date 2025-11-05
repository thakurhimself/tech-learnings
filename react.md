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


