## There Are No Environments
_Joel Tosi_

### Intro
* Eventually stopped talking environments because the discussion became an impediment
* Just assume there are no environments
* Dude's Law
    * Less about the how, more about the why
* Project -> Process -> Product
    * Projects are a list of things to do
    * Process gets wrapped around projects to get them to completion
    * Often times, less process and noise is needed
    * A project is when you build what people want
    * A product is when you build what people need
    * Products are meaningful
* If you're writing bad code, building it faster won't help

### What about environments?
* The things you push your stuff through
* At CME, had six Q/A environments because they did lots of projects
    * Just built bad code faster
    * Building an environment is *familiar*
* What assumptions did you make that got you into this bad situation?
    * *Double-loop learning explained -- review slides/video*
    * Single loop learning is most common, improving a system as it exists
    * Double loop learning means asking the right questoins to break assumptions/values/beliefs
    * Go back to "why we do what we do", not "what we do"
* **Software can't fix software problems**
* We stopped learning about products and maintenance

### Simplify before automating
* Simplify reporting structure, motivations, etc
* What we say: Dev -> QA -> UAT -> Prod
* Reality: It's a big mess
    * Nothing is linear -- people and teams aren't linear so how can testing be done linearly?
    * Start removing environments
    * *See chart "How to remove" from slides/video*
    * People work together in organizations but they don't trust each other -- try trusting your people
    * Have better conversations with employees
* Developers need context of **why** they're building something
    * Helps with pushing responsibility to teams that do the work
* **Every step between your product and customer separates and muddies learning**
* **Drive outcomes (experiences) and reduce output (environments)**
* Most orgs are designed for **problem resolution** and not **value creation**

### Testable over testing
* Testability is a deep debt
* You should be able to do demos of new features/fixes
* Tests should be isolated/independent
* Doubt grows when systems can't be tested
    * Can't solve doubt with more software

### Grow deeper product context
* Ask better questions to get context
* Most places are concerned with efficiency, not effectiveness
* Work on one thing between multiple groups so you can finish it
* Four teams working on 28 initiatives won't work
    * Nobody could work together
    * Prioritization meetings start cropping up
    * Agile development without output isn't effective
    * Reduce the number of simultaneous initiatives
* Need to build a common language within an organization
    * Product ideas flow thrugh a system to become product choices
    * *Refer to "Building common language" slide from slides/video*
* Know who you're building a product for -- otherwise you add more software to the problem
    * Software can't fix not knowing your customers' needs/wants
    * Environment sprawl takes over along with complexity
* **Make intentional decisions over accidental discoveries**
    * **Embrace learning and avoid misunderstanding**
* **Good architecture means components are cheap to change**
* **DevOps without product context is at best the wrong thing righter**

### Own your world
* Victim card -- get out of responsibility free
* *"It's not my fault, it's their fault"*
* What can you do with failures outside your control?
    * Rethink about how you're building things
    * Run smoke tests to verify dependencies are working properly
    * Find a way to take more resposibility
* **People talking over committees decreeing**
    * *Committees are horrible things*
    * Be clear on values/direction
    * Let people doing the work solve the problems
    * Provide those people with data
    * Tell then what success looks like and then let them figure out how to do it
    * Patterns emerge over time
* Don't promote/maintain through environments
    * Pull through examples, tests and learning
    * *"I don't care about environments, I care about the product"*
    * Spend time asking people what they need
* Experiment and learn
    * Are previous assumptions still valid?
    * **What is stopping you?** 
    * Sales doesn't sell you **what you need**, they sell you **what they want you to have**
