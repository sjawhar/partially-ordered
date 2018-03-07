# Tips for Writing Testable Loopback Apps

When it comes writing RESTful APIs, I like [Loopback](https://loopback.io/). It's based on [Express](https://expressjs.com/) and benefits from the rich ecosystem of express middleware. What Loopback adds is model logic, making it really easy to define and consume resources and relationships. Don't get me wrong, it has its problems (which Loopback 4 seems to be tring to address) - but it's a great way to get off the ground quickly with little effort. Except for when it comes to testing, that is...

## The Problem

I'm sure you follow the best TDD practices and have 100% code coverage on all your project, right? If so, you should be aware that what makes Loopback so great can also make it pretty easy to write code that's pretty hard to test. For example, you can use the CLI to scaffold a loopback app with models and relationships without writing any code. At that point, your repo will mostly be a collection of model-definition JSON files. Great, right?! Except... how do you test JSON files? Oh, I see a User.js file there, I'll just import it into my test and... nope, that file is used during that Loopback boot process to add logic to the model, but it is not the model itself. "How in the seven hells do I test this thing?!" you're saying. Don't worry! By being thoughtful about how we structure our app and disciplined in our testing, everything will work out.

## Example: User management app with slack webhook
* Introduce the example
* Writing a slack bot without calling slack

## The Design
Let's get started by writing out our app's expected behavior, at least at a high level. In other words, before we get into models, events, and all that jazz, let's answer the question "what does the app *do*?"

* Introduce ava
* Start with some todo tests
* Now start designing needed resources, properties, and relationships
    * Which relationships are you expecting to use? Write tests to ensure you can!
* Write tests that ensure you've defined your resources correctly
    * test/models/User/definition.js
    * Tests
        * the model exists (yes, really)
        * Properties (names, types, required)
        * Model-specific overrides (like http path)
* Testing relations -> Integration tests
* Put it together: functional tests -> Supertest

# The Scaffolding
* Using CLI to scaffold app
* Ditch their directory structure and use src/ and test/
    * You're probably not putting your client and server in the same repo anyway
* Update the tests to work with the scaffolded apps

## The Logic
So we've scaffolded our app, defined our models and relations, and set up a clean directory structure. Now let's start adding some logic.
* Team member role
* Define expected behavior of team member role
* Write tests
    * Assume that you will be able to import the role resolver and test it directly
* Write role logic as importable module
* Directory structure
    * modular subdirectories
        * boot
        * constants
        * listeners
        * models
        * remote-hooks
        * remote-methods
        * roles
        * utils
    * mirror src/ and test/
* Balance unit tests and integration tests
* Dependency injection
    * Stubs and spies: using sinon

## The Tools
* Setting up docker-compose files
* docker-compose.test.yml
    * Derive your test image from your production image
* Dockerized database
    * In-memory database pitfalls
    * Different connectors behave differently, want to test actual behavior

## The Issues
* Generating libraries: use docker images!
* Callback-only functions: util.promisify
* Supertest rejected promises
