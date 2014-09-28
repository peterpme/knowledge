# Behind The Scenes of A Browser
Disclaimer: Still in brain dump mode

There's a lot that goes into actually having something appear on your website. The browser can request multiple forms of content whether it would be HTML, CSS, JS, images, other types of content, etc.

> Browsers like Chrome make `predictive optimizations` by observing network traffic. What it'll do is build a predictive model in order to improve performance.
> What that means is the second you start looking for a certain URL, like Facebook, Chrome will dispatch a speculative DNS lookup and even start the TCP handshake before you hit `enter`

While browsers will have different engines that handle rendering differently, there are simlarities. They will have a User Interface, Browser Engine, Rendering Engine, Network Engine, Javascript Engine and the UI backend.

- `User Interface` - What the User sees. Includes Address Bar, Search, Bookmarks, etc.
- `Browser Engines` - Middleman between user interface and the rendering engine
- `Renderring Engine` - Takes care of the page rendering process
- `Network Engine` - Handles network requests like HTTP and other requests
- `UI Backend` - draws basic widgets and exposes generic interface that is not platform specific
- `Javascript Interpreter` - Parses and executes JS
- `Data Storage` - Persistence layer to store stuff like cookies and now things like `localStorage`, `IndexedDB`, `WebSQL` and `FileSystem`

Once you initiate the TCP handshake with the browser, the network engine will request 8kb of data at a time. All modern browsers are multi-threaded, which means they have the abililty to download in paralell, usually around 6 streams but there are ways an individual can increase this limit.

From this point on the HTML parsing begins, which is an interative process. What that means is that a web page will start loading before it's completely finished. The reason this works the way it does is to improve the User Experience. The parser asks the lexer for a token and if it's valid it appends it to the `parse tree`. If it's not valid, that means there were errors in the process and document is not valid. There are many situations a document can become invalidated

`Parsing` - The process of converting human readable code into something the computer can use.

`Lexical Analysis` - Breaks input into `tokens`, the building blocks of the specific ruleset
`Syntax Analysis` - The process of applying those rules

`Lexer` - Knows how to stripe unwanted information like whitespace and line breaks
`Parser` - Constructs `parse tree` based on the rules provided

There are different kinds of parsers.

`Top Down` - High level overview approach that looks for a match
`Bottom Up` - Processes input after input and transforms them into syntax rules until all rules are met

`Webkit` uses `Flex` for the Lexer and `Bison` for the Parser

There are huge differences between parsing HTML and everything else. The reason is because humans make enormous mistakes. CSS and JS use context free grammar. The human is an example of what is NOT a context free grammar and neither is HTML. HTML has soft syntax which means not all elements need to close, elements can be created on the fly (but not necessarily allowed).

HTML needs to be more forgiving me because of its need to render out the page error-free (in terms of what the user sees). Browsers have very advanced error handling techniques that will close unclosed brackets, move elements that aren't syntatically correct and default made-up elements with real ones. Eg, `<myblock>` would become a normal `<div>`

`Parse Tree` - Collection of `DOM` elements and nodes
`DOM` - Document Object Model. Objective representation and interface of HTML representation
`Document` - root of Parse Tree
`Context-Free Grammar` - specific rules and follow a specific format.

HTML can't use a top down or bottom up parser because of potentially unknown factors. Document.write() can be called once javascript is executed that could totally void what previously took place. The HTML has the ability to change on the fly and that's how the system is meant to work.

The tokenization of HTMl elements is the lexical analysis.

What happens is that the second before the browser starts parsing the HTML it enters a `data state`. As soon as it sees a `<` it will enter a `TAG OPEN STATE` and individual [a-z] characters are parsed. The browser is now looking for a specific element like `<html>`. It's in a `TAG NAME STATE` until it reaches the final `>`  As soon as `<HTML>` is created, the browser is back in data state until it reaches another `<` to start the process over again. Once the browser hits a `/` it will enter a `TAG CLOSE STATE` to close out the tag `</HTML>

## Resources
- [http://www.html5rocks.com/en/tutorials/internals/howbrowserswork/](http://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)
- [http://www.aosabook.org/en/posa/high-performance-networking-in-chrome.html](http://www.aosabook.org/en/posa/high-performance-networking-in-chrome.html)