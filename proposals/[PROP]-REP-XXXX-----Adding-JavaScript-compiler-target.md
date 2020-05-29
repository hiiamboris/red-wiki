<table>
  <tr>
    <td>REP:</td>
    <td>XXXX</td>
  </tr>
  <tr>
    <td>Title:</td>
    <td>Adding JavaScript compiler target</td>
  </tr>
  <tr>
    <td>Author(s):</td>
    <td>Donald Tsang</td>
  </tr>
  <tr>
    <td>Status:</td>
    <td>Draft</td>
  </tr>
  <tr>
    <td>Date Created:</td>
    <td>?????</td>
  </tr>
  <tr>
    <td>Date Last Actioned:</td>
    <td></td>
  </tr>
</table>

# Benefits

* being able to use Node.js with its extensive set of ready to use frameworks for creation of web servers and other stuff. Targeting Android natively is good, but as it is a language does need extensive framework developers' support and the Red community obviously can't provide that kind of support to the extent of the JavaScript community with active members like Facebook. Targeting JavaScript would allow to "cheat" and use those frameworks in the manner CoffeScript or Dart uses them without having to re-implement anything in Red
* consequently the availability of miscellaneous useful frameworks like React, Ramda, Underscore, AngularJS etc. in Red would allow to target web and mobile development to the full extent, which is a really hot topic now, and  features of Red may be appealing to web developers and improve their experience
* having access to familiar libs and frameworks would smooth the learning and transition curve for those who wants to try Red, but wary
of time costs and technical risks of diving completely into some unknown technology. Red syntax itself is pretty tiny, so it would be enough for them to learn how to use already familiar frameworks in Red and explore new opportunities Red gives thereafter.
* being able to add RedJS to NPM repository, thus simplifying and providing access to the language in a familiar way for developers

# Challenges
* optimally mapping Red objects, function call protocols etc. to JavaScript so as to simplify the use of JavaScript frameworks without additional overhead
* mapping Red implemented functions and objects back to JavaScript code so as to simplify their use in other languages and JavaScript itself
* making the Red compiler emitted JavaScript code easily readable the way CoffeeScript does 
* efficiently mapping Red built-in vocabulary to JavaScript

# Questions
* Is it possible to include a wider variety of JS frameworks and libraries?
* If JS has integration, will XSLT, XML and CSS have integration too?
* (see https://github.com/red/red/wiki/%5BPROP%5D-REP-XXXX---Adding-other-Red-dialects for details)