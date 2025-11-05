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

1. ""Group related state"": if you always update two or more state variables
	consider merging them into a single state.

2. ""Avoid contradictions in state"": When the state is structured in a way 
	that several pieces of state may contradict and disagree with each other,
	you leave room for mistakes. Try to avoid this.

3. ""Avoid redendant state"": if you calculate some information from the 
	component's props or its existing state variables during rendering, you
	should not put that information into that component's state.

4. ""Avoid duplication in state"": When the same data is duplicated between
	multiple state variables, or within nested objects, it is difficult to 
	keep them in sync. Reduce duplication when you can.

5. ""Avoid deeply nested state"": Deeply hierarchical state is not very 
	convenient to update. When possible, prefer to structure state in a flat
	way.

The goal behind these principles is to make state easy to update without 
introducing mistakes. *Make you state as simple as it can be - but simpler*


