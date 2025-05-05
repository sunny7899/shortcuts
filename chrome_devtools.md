Command Line API debugging Tool, chrome shortcuts

Here i will talk about dev tools that we use generally use while web development. Following are some command-line methods and tricks that are very helpful. Here I will also talk about chrome browser shortcuts.

Command Line API:

$-  

It returns an array of elements that match the given CSS selector.

It is an alias for document.querySelector()

var images = $('img');
 for (each in images) {
   console.log(images[each].src);
 }

$$-

$$() is an alias for document.querySelectorAll()

$$("header")

$x-

$x("//p")
$x("//p[a]")
$x("//*");  It will select all the elements that is present in document.

In this we put HTML tag

$x(".//header")

$0  

The currently-selected object in the inspector.

$_

The previously evaluated statement

$1

The previously-selected object in the inspector.

$0, $1, $2, $3 and $4 – give last 5 searches

$n(index)  

Access to an array type of the last 5 inspected elements.

dir(object)

Prints an interactive listing of all fields of the object. This looks the same as the view that you would see in the DOM tab.

dirxml(node)

Prints the Extensible Markup Language(XML) source tree of an HTML or XML element. This looks equal to the view that you would see in the HTML tab. You can click on a node to inspect it in the HTML tab.

clear()

Clears the console.

copy()

Copies everything passed to it to the clipboard.

inspect-

inspect(object[, tabName])  

Inspects an object in the most suitable tab or the tab identified by the optional argument tabName.

inspect(document.body);

keys(object)  

Get the particular keys and values of an object-

values(object)  

Returns an array containing the names of all properties of the object.

Example-

var player1 = { "name": "Tom", "level": 42 }

keys(player1)

values(player1)

monitorEvents

monitorEvents(object[, types])  

It turns on logging for all events dispatched to an object. The optional argument types may specify a partcular family of events to log. The most commonly used values for types are "mouse" and "key". The full list of available types includes "composition", "contextmenu", "drag", "focus", "form", "key", "load", "mouse", "mutation", "paint", "scroll", "text", "ui", and "xul".

monitorEvents($0, "key");

monitorEvents(window, "resize");

unmonitorEvents(object[, types])  

It turns off logging for all events dispatched to an object.

monitor

function sum(x, y) {
   return x + y;
 }
 monitor(sum);
 sum(1,2)

performance-  

performance.timing  

performance.memory  

performance.navigation

 To Debug DOM modifications-

Right-click a component/element and enable Break on Subtree Modifications.

getEventListeners(document)

Tip: For any other help related to browser go to the Search bar of chrome browser and search-

about: about or chrome://chrome-URLs - It Displays all the chrome links.

about: version - Display information about the browser.

Manual editing CSS in browser console-

 function makeGreen() {
  const p = document.querySelector('p');
  p.style.color = '#BADA55';
  p.style.fontSize = '50px';
 }

Following shortcuts, you can use to open different tabs in a chrome browser for debugging-

1. Ctrl + E - To directly navigate to the address bar.

2. Ctrl + L - Used to highlight all text in the address bar.

3. Ctrl + J – To open the downloads tab.

4. Ctrl + T-  To open a new tab.

5. Ctrl + Shift + T- To again open the currently closed tabs.

6. F12- To open the console tab.

7. ctrl+shift+c -Redirect to elements chrome dev tools

8. ctrl+shift+p - works on chrome dev tool to clear the console, change theme etc.

9. ctrl+shift+i -Redirect to console chrome dev tools

10.  ctrl+shift+m - To open responsive mode chrome developers tool

11.  F8-continue

12.  F10-step over

13.  F11-step into

14.  shift+F10-step out

https://developers.google.com/web/tools/chrome-devtools/console/utilities

Browser Dev tools
CSS Panel
Computed panel
Grid and flexbox tools