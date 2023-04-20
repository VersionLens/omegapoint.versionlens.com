Code-changes by AI must be "scoped"<br> - structurally restricted.



A Scope for changes should consist of:
1. A whitelist of what is allowed to be changed <!-- .element: class="fragment" -->
1. Guidance examples for how changes should be made <!-- .element: class="fragment" -->



An example Scope for making markup changes in a frontend app:

1. What files can be changed? <br>`src/**/*.jsx` <!-- .element: class="fragment" -->
2. Guidance examples for how changes should be made: <!-- .element: class="fragment" -->



*Guidance example [1/3]*
| | |
| --- | --- |
| Change Request | Make the 'Add to basket' button more prominent |
| Look in the file | `src/components/AddToBasket.jsx` |
| Find the element matching this description | The 'Add to basket' button |
| Action | Make the 'Add to basket' button more prominent by increasing its size using a Tailwind class |
<!-- .element: style="font-size: 30px;" -->



*Guidance example [2/3]*
| | |
| --- | --- |
| Change Request | Add the description to the list of products in the products list |
| Look in the file | `src/components/ProductList.jsx` |
| Find the element matching this description | One product in the list of products |
| Action | Add the product description to the element by adding a `<p>` element with the description |
<!-- .element: style="font-size: 30px;" -->



*Guidance example [3/3]*
| | |
| --- | --- |
| Change Request | Add a link to 'About us' with target '/about-us' to the footer |
| Look in the file | `src/components/Footer.jsx` |
| Find the element matching this description | The footer |
| Action | Add a link to 'About us' to the footer by adding a `<a>` element with a link to '/about-us' |
<!-- .element: style="font-size: 30px;" -->



The AI is then given a real Change Request, parsed from a natural language chat with a non-developer:

| | |
| --- | --- |
| Change Request | Make the main heading say 'Welcome to the Version Store' |
<!-- .element: style="font-size: 30px;" -->



The file to change is found from a semantic search over the whitelisted files.
| | |
| --- | --- |
| Change Request | Make the main heading say 'Welcome to the Version Store' |
| Look in the file | `src/components/Header.jsx`<!-- .element: class="fragment" --> | 
<!-- .element: style="font-size: 30px;" -->



The description is suggested by the AI given the current Change Request and the guidance examples.
| | |
| --- | --- |
| Change Request | Make the main heading say 'Welcome to the Version Store' |
| Look in the file | `src/components/Header.jsx` | 
| Find the element matching this description | The main heading<!-- .element: class="fragment" --> |
<!-- .element: style="font-size: 30px;" -->



The change is suggested by the AI given the current Change Request, the element found in the previous step, and the guidance examples.
| | |
| --- | --- |
| Change Request | Make the main heading say 'Welcome to the Version Store' |
| Look in the file | `src/components/Header.jsx` | 
| Find the element matching this description | The main heading |
| Action | Make the main heading say 'Welcome to the Version Store' by changing the text inside the h1 element<!-- .element: class="fragment" --> |
<!-- .element: style="font-size: 30px;" -->



Sounds cool, but what if an adversarial prompt (or hallucination) makes the AI insert this:
```
require('child_process').exec('rm -rf /')
```
<!-- .element: style="text-align: center" -->



When the changed source file is evaluated, we are effectively running arbitrary, user-generated code.

We should deal with this the same way we deal with any other user-generated code: by running it in a sandbox. <!-- .element: class="fragment" -->



![Scoped change Sandbox](/images/just-sandbox.png "Scoped change sandbox")



Each Sandbox could be a Kubernetes namespace running specific SHAs of all app services and databases, with source code in hot-reload mode.

We call these Sandboxes "Versions" of a full-stack app, and run them on demand in our cloud <!-- .element: class="fragment" -->



How do we verify that changes made by the AI<br> were correct?

![Scoped change Sandbox](/images/just-pr.png "Scoped change PR")<!-- .element: class="fragment" -->



How do we verify that changes made by the AI<br> were correct?
1. Immediate manual verification by the non-developer after hot-reload <!-- .element: class="fragment" -->
1. Automated tests on git commit to task branch. <!-- .element: class="fragment" -->
1. Human developer review of task branch Pull Request <!-- .element: class="fragment" -->



Measuring success
* Success metric for AI-driven product changes: number of accepted Pull Requests <!-- .element: class="fragment" -->
* We ask GPT-4 to verify that all changes made by the AI seem to follow the guidance examples <!-- .element: class="fragment" -->
* We manually verify that a random subset of changes made by the AI actually follow the guidance examples <!-- .element: class="fragment" -->



A system that learns
![Scoped change Sandbox](/images/pr-guidance.png "Scoped change learning")<!-- .element: class="fragment" style="max-width: 65%;" -->