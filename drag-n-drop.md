# Drag and drop feature in web and mobile applications

HTML drag and drop(Dnd) api enables drag and drop features in browsers.
[HTML drag and drop API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API)

## Concept and usage
### Drag events
The Dnd uses the DOM event model [ref](https://developer.mozilla.org/en-US/docs/Web/API/Event) and drag event events inherited from mouse events.
A typical drag operation begins when a user selects a ""draggable"" element to a 
droppable element, and ends when the user releases the dragged elements.

During drag operaiont, several events fire, and some events might fire many times,
such as the drag and dragover events.

Each drag event has associated event handlers:
drag - a dragged item is dragged.
dragend - a drag operation ends.
dragenter - a dragged item enters a valid drop target.
dragleave - a dragged item leaves a valid drop target.
dragover - a dragged items is being dragged over a valid drop target, every few 100
			ms.
dragstart - the user starts dragging an item.
drop - an item is dropped on a valid drop target.

Note - Neither a dragstart nor dragend events are fired when dragging a file into the
browser from the OS.

### The basics
Basic steps to add drag-and-drop functionality to an application.

IDENTIFY WHAT IS DRAGGABLE
Making and element draggable requires adding the draggable attribute and the dragstart
event handler, as :
``
	HTML
	---
	<p id="p1" draggable="true"> Draggable element </p>
``

``
	JS
	---
	const element = document.getElementById("p1");
	element.addEventListener("dragstart", (ev) => {
		// Add the target element's id to the data transfer object
		ev.dataTransfer.setData("text/plain", ev.target.id);
	}
``

### DEFINE THE DRAG'S DATA
The application is free to include any number of data items in a drag operation. Each
data item is a string of a particular type - typically a MIME type such as text/html.

Each DragEvent has a dataTransfer property that holds the event's data. This property
(which is a DataTransfer object) also has methods to manage drag data. The setData() 
method is used to add an item to the drag data, as shown in the following example:

`` 
	JS
	---
	function dragstartHandler(ev) {
		// Add differnt types of drag data
		ev.dataTransfer.setData("text/plain", ev.target.innerText);
		ev.dataTransfer.setData("text/html", ev.target.outerHTML);
		ev.dataTransfer.setData(
		"text/uri-list",
		ev.target.ownerDocument.location.href,
		);
	}
``

Note - for recommeneded drag types and Drag data see official ref main ref.

### DEFINE THE DRAG IMAGE
By default, the browser supplies an image that appears beside the pointer during a 
drag operation. However, an application may define a custom image with the 
setDragImage() method, as shown in the following example.

``
	JS
	---
	// Create an image and then use it for the drag image.
	// NOTE: change "example.gif" to a real image URL or the image
	// will not be created and the default drag image will be used
	let img = new Image();
	img.src = "example.gif";
	function dragstartHandler(ev) {
		ev.dataTransfer.setDragImage(img, 10, 10);
	}
``

### DEFINE THE DROP EFFECT
The dropEffect property is used to control the feedback the user is given during a
drag-and-drop operation. It typically affects which cursor the browser displays while
draggin. For example, when the user hovers over a drop target, the browser's cursor
may indicate the type of operation that will occur.

Three effects may be defined:
1. 'copy' indicates thats the dragged data will be copied from its present location
	to the drop location.
2. 'move' indicates that the dragged data will be moved from its present location to
	the drop location.
3. 'link' indicates that some form of relationship or connection will be created 
	between the source and drop locations.

During the drag operation, drag effects may be modified to indicate that certain 
effects are allowed at certain locations.

The following example shows how to use his property:
``
	JS
	---
	function dragstartHandler(ev) {
		ev.dataTransfer.dropEffect = "copy";
	}
``

see docs for more.

### DEFINE A DROP ZONE
By default, the browser prevents anything from happening when dropping somthing into
most HTML elements. To change that behavior so that an element becomes a ""drop zone""
or is ""droppable"", the element must listen to both dragover and drop events.

Example:
``
	HTML
	---
	<p id="target"> Drop Zone </p>
``

``
	JS
	---
	const target = document.getElementById("target");

	target.addEventListener("dragover", (ev) => {
		ev.preventDefault();
		ev.dataTransfer.dropEffect = "move";
	})

	target.addEventListener("drop", (ev) => {
		ev.preventDefault();
		// Get the id of the target and add the moved element to the target's DOM
		const data = ev.dataTransfer.getData("text/plain");
		ev.target.appendChild(document.getElementById(data));
	})
``

Note that each handler calls preventDefault() to prevent additional event processing
for this event (such as touch events or pointer events).

### HANDLE THE DROP EFFECT
The handler for the drop event is free to process the drag data in an application-
specific way.

Typically, an application uses the getData() method to retrieve drag items and then
process them accordingly. Additionally, application semantics may differ depending
on the value of the ""dropEffect"" and/or state of modifier keys.

The following example shows a drop handler getting the source elements's id from the
drag data, and then using the "id" to move the source element to the drop element:
``
	HTML
	---
	<p id="p1" draggable="true"> Draggable element </p>
	<div id="target"> Drop Zone </div>

	JS
	---
	const source = document.getElementById("p1");
	const target = document.getElementById("target");

	source.addEventListner("dragstart", (ev) => {
		// Add the target element's id to the data transfer object
		ev.dataTransfer.setData("application/my-app", ev.target.id);
		ev.dataTransfer.effectAllowed = "move";
	}

	target.addEventListner("dragover", (ev) => {
		ev.preventDefault();
		ev.dataTransfer.dropEffect = "move";
	}

	target.addEventListner("drop", (ev) => {
		ev.preventDefault();
		// Get the id of the target and add the moved element to the target's DOM
		const data = ev.dataTransfer.getData("applicatin/my-app");
		ev.target.appendChild(document.getElementById(data));
	}
``
see docs for more

### DRAG END
At the end of a drag operation, the ""dragend"" event fires at the source element -
the element that was the target of the drag start.

This event fires regardless of whether the drag completed or was canceled. The 
""dragend"" event handler can check the value of the dropEffect property to determine
if the drag operation succeeded or not.

see docs for more info

## Interfaces
The HTML Drag and Drop interfaces are "DragEvent", "DataTransfer", "DataTransferItem",
 and "DataTransferItemList".

 The DragEvent interface has a constructor and one dataTransfer property, which is a
 DataTransfer object.

 DataTransfer objects include the drag event's state, such as the type of drag being
 done (like copy or move), the drag's data (one or more items), and the MIME type of
 each ""drag item"". DataTransfer objects also have methods to add or remove items to
 the drag's data.

 The DragEvent and DataTransfer interfaces should be the only ones needed to add HTML
 Drag and Drop capabilities to an application.

 Each DataTransfer object contains an items property, which is a list of 
 DataTransferItem objects. A DataTransferItem object represents a single drop item, 
 each with a kind property (either string or file) and a type property for the data
 item's MIME type. The DataTransferItem object also has methods to get the drag item's
 data.

 The DataTransferItemList object is a list of DataTransferItem objects. The list 
 objects has methods to add a drag item to the list, remove a drag item from the list,
 and clear the list of all drag items.

 A key difference between the DataTransfer and DataTransferItem interfaces is that the
 former uses the synchronous getData() method to access a drag item's data, but the
 latter instead uses the asynchronous getAsString() method.


