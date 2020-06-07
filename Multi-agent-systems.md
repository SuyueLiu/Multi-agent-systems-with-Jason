# The Jason Agent programming Language 

```
Define
    -+: remove a former instance(if any exist) while adding the new one.
    @g/@h: the label/name of a plan.
    +b : belief addition
    -b : belief deletion
    +!g : achievemen-goal addition
    -!g : achievemen-goal deletion
    +?g : test-goal addition
    -?g : test-goal deletion
    ~ : strong negation
```

Agent architecture: the software framework within which an agent program runs, such as the PRS. The plans are the program that inhabits this architecture.

Jason is an extension of AgentSpeak, which is based on the BDI architecture.

Goals are the achieved by the execution of plans.

## Beliefs

**Beliefs** represent the information available to an agent (about the environment or other agents).

​	`tall(john)`

Which expresses a particular **property** - that of being *tall*  - of an object or individual.  To represent the fact that a certain relationship holds between two or more objects, we can use a *predicate* such as:

​	`likes(john, music)`

![basis](https://github.com/SuyueLiu/Multi-agent-systems-with-Jason/blob/master/basis.PNG)

* Atom: any symbol starts with a lowercase letter, which is used to represent particular individuals or objects. They are equivalent of 'constant' in the first-order logic.
* Logical variable: a symbol starts with an uppercase letter.
* Constant: atoms, numbers, strings 
* Structure: used to represent complex data, for example, it could be used to represent the information of a member in the university (name, ID, course...). Structure starts with 
* Annotations: those complex terms that provide details that are strongly associated with one particular belief, annotations are enclosed in square brackets immediately following a literal, such as `busy(john)[expires(autumn)].` 

Initially variables are free or uninstantiated and once instantiated or bound to a particular value, they maintain that value throughout their scope − in the case of AgentSpeak, we shall see that the scope is the **plan** in which they appear. Variables are bound to values by unification ; a formula is called ground when it has no more uninstantiated variables. Using the example above, the variable **Person** could be bound to the atom **John**. More generally, the variable can be bound to a **constant** or **structure**.

Note that the **annotation** `expires(autumn)` is giving us further information about the `busy(john)` **belief** in particular. It is equal to write something like: `busy_until(John, autumn)`. The `expires` annotation means absolutely noting to the **Jason** interpreter. But there are other belief annotations which do have specific meanings for the interpreter. Such as `source` annotation is used to record what was the source of information leading to a particular belief. 

There are three different types of information **source** for agents in MAS:

* **perceptual information:** an agent acquires certain beliefs as a consequence of the (repeated) <font color = 'red'>sensing of its environment </font> − a symbolic representation of a property of the environment as perceived by an agent is called a *percept* ;
* **communication:** as agents communicate with other agents in a multi-agent system, they need to be able to <font color='red'>represent the information they acquired from other agents</font>, and it is often useful to know exactly which agent provided each piece of information;
* **mental notes:** <font color='red'>things that agent itself will need to remember in certain future circumstances.</font> it makes certain programming tasks easier if agents are able to remind themselves of things that happened in the past, or things the agent has done or promised: beliefs of this type are added to the belief base by the agent itself as part of an executing plan, and are used to facilitate the execution of plans that might be selected at a later stage. <font color= 'red'>Annotated by `source[self]` which represent that the belief is add by agent itself.</font>

## Strong negation

* **Closed world assumption**: Anything that is neither known to be true, nor derivable from the known facts using the rules in the program, is assumed to be false.
* `not` operator is such that the negation of a formula is true if the interpreter *fails* to derive the formula using the facts and rules in the program.
* **Strong negation**: denoted by `~` operator to express that an agent *explicitly believes something* to be fails.

## Rules

Rules allow us to conclude new things based on things we already known.

```
likely_color(C,B)
	:- color(C,B)[source(S)] & (S == self | S == percept).
```

* To the left of the operator `:-`, there can be only one literal, which is the conclusion to be made if the condition to the right is satisfied (according to the agent's current beliefs).
* Initial beliefs/rules tell the interpreter which beliefs/rules should be in the belief base or rules base when the agent first starts running. 
* Unless specified otherwise, all initial beliefs are assumed to be **"Mental Note"** which represented as `[source(self)]` in the source code.

## Goal

Whereas beliefs, in particular those if a perceptual source, express properties that are believable to be true of the world in which the agent is suited, <font color='red'>goals express the properties of the states of the world that the agent wishes to bring about.</font>

In AgentSpeak, there are two types of goals: **achievable goal** and **test goals**.

* **Achievable goals:** the goal denoted by the `'!'` operator. For example, we can say `! own(house)` to mean that the agent has the goal of achieving a certain state of affairs in which the agent will believe it owns a house, which probably implies that the agents does not currently believe that `own(house)` is true. 
* **Test goal**: the goal denoted by the `'?'` operator, which used to check whether the agent believes a literal or conjunction literal. Test goals are used simply to <font color='red'>retrieve information that is available in the agent's belief base.</font> For example, `? Bank_balance(BB)`, that is typically because we want the logical variable `BB` to be instantiated with the specific among of money the agent currently **believes** its bank balance is.

In practice, we only provide the beliefs that we wish the agent to have at the moment it first starts running, we can also provide goals that the agent will attempt to achieve from the start, if any such goals are necessary for a particular agent. That is, we can provide **initial goals** to our agent. 

`! clean(house)`...

For example, this is an agent that would start running with the long term goal of getting the house clean.

## Plans 

An agent reacts to **events** by execution plans, events happen as a consequence to changes in the agent's beliefs and goals. An AgentSpeak plan has three distinct parts: the triggering event, the context, and the body. The triggering event and the context are called *head* of the plan. 

`triggering event: context <- body.`

### Triggering event

As we know, the agent has an **initial goal** that they try to achieve in the long term, determine the agent's **pro-active behavior**. While acting so as to achieve their goals, agents need to be attentive to changes in their environment, because those changes can determine whether agents will be effective in achieving their goals and indeed how efficient they will be in doing so.

There are two types of changes: **changes in beliefs** (which refers to the information agents have about their environment or other agents) and **changes in the agent's goals**. Changes in both types of attitudes create, within the agent's architecture, the **events** upon which agents will act. 

* **Trigger event**: if an event that took place matches the triggering event of a plan, that plan might start to execute.
* **Relevant**: if the triggering event of a plan matches a particular event, we say that the plan is *relevant* for that particular event.
* **Event**: represents changes in beliefs and goals, such changes (beliefs and goals) can be of two types: *addition and deletion*.
* Events for belief additions and deletions happen, for example, when agent updates its beliefs according to its perception of the environment obtained at every reasoning cycle. 
* Events due to the agent having new goals happen mostly as a consequence of the execution of other plans, but also as a consequence of agent communication. 
* The goal deletion types of events are used for handling plan failure.

Suppose we want to develop an agent that may need to have the goal of serving dinner, which implies that the goal will have been achieved when the agent believes that dinner is served, represented as `served(dinner)`. We need a plan that the agent will start executing when it comes to have the goal of achieving a state of affairs where the agent will have such a belief, for this, the plan will have a triggering event such as `+! served(dinner)`. More  generally, the agent may have to serve different meals, in this case, the trigger event would probably look like `+! served(Meal)`, so that    if the agent has a new goal to have a  particular *dinner* served , then the plan would be executed with the logical variable **Meal** instantiated with **dinner**.

### Context 

The context of a plan is used precisely for checking the current situation so as to determine whether a particular plan, among the various alternative ones, is likely to succeed in handling the event (e.g. achieving a goal), giving the latest information the agent has about its environment. A plan that has a context which evaluates as true given the agent's current beliefs is said to be applicable as that moment in time, and is a candidate for exclusion.

It is often the case that a context is simply a conjunction of **default literals** and **relational expression**.

* A literal is a predicate about some state of world, which may have a strong negation.
* A default literal is a literal which may optionally have another type of negation, known in logic programming as *default negation*, which is denoted by the **not** operator.
* Any literal appearing in the context is to be checked against the agent's belief base.

![conjunction](https://github.com/SuyueLiu/Multi-agent-systems-with-Jason/blob/master/conjunction.PNG)

```
+!G[source(baddy)] : true <- true.
```

The plan tells the agent to ignore any goals that agent 'baddy' tries to delegate. The key word `true` can be denote an empty context, and also empty body. Alternatively, we can omit those parts of a plan if they are empty. Thus,  `+!G[source(baddy)]`.



### Body

The body of a plan defines a course of action for the agent to take when an event that matches the plan's triggering event has happened and the context of the plan is true in accordance with the agent's beliefs (and the plan is chosen for execution ). There are six different types of formula that can appear in a plan body.

#### Actions

Actions represent the things the agent is capable of doing, we will then need symbolic representations of these actions.

The language construct for symbolically referring to such actions is simply a *ground predicate*. That is, we need to make sure that any variables used in the action become instantiated before the action is executed.

#### Achievement goals

The way an achievement goal to be added to the agent’s current goals is denoted is by prefixing the literal with the `!` operator.

If we include a goal in a plan body, a plan needs to be executed for that goal to be achieved, and only after the plan has successfully executed can the current course of action be resumed. For example, if we have `a1; !g2; a3` in a plan body, the agent will do a1, then it will have a goal of achieving `g2`, which will involve some other plan being executed, and only when that plan is finished can `a3` be executed.  

A plan body is a *sequence* of formula to be executed. At times, it may be the case that we want the agent to have another goal to achieve but we do not need to wait until this has actually been achieved before we can carry on with a current course of action. The way to express this is to use the `!!` operator. For example, if the agent has `!at(home); call(John)` as part of a plan, it will only call John when it gets home, whereas if it is `!!at(home); call(John)`, the agent will call John soon after it comes to have a new goal of being at home, possibly even before it does any action in order to achieve that goal.

#### Test goals

The context should be used only to determine of the plan is applicable or not(i.e. whether the course of action has a chance of succeeding), practically we may need to get the latest information which might only be available, or indeed might be more recently updated, when it is really necessary. For example, if the agent is guaranteed to have some information about a target’s coordinates, which are constantly updated, and only needed before a last action in the course of action defined by the plan body, then it would make more sense to use `?coords(Target,X,Y)` ; just before that action to include `coords(Target,X,Y)` in the context.

#### Mental notes

An agent needs a **mental note** (beliefs) to remind itself of something it (or some other agent ) has done in the past, or to have information about an interrupted task that may need to be resumed later, or a promise/commitment it has made. For example, an agent needs to remember when the lawn was last mowed; this is done in the plan body with a formula `+mowed(lawn, Today)`. 

A downside is that we need to delete unnecessary mental notes, such as `-needs_be_done(mowing(lawn))`. In some cases we only need the last instance of a certain mental note in the belief base, the operator `-+` can be used to remove a former instance while adding the new one. 

* `-`operator attempting to delete a belief that does not exist is simply ignored, it does not cause a plan failure.
* `_` is the anonymous variable, it can unify with any value, and is useful where the value is irrelevant for the rest of the plan.

#### Internal actions

The way an internal action is distinguished from (environment) actions is by having a `'.'` character within the action name. A standard internal action is denoted by an empty library name.

* `.send`: requires three parameters: the name of the agent that should receive the message, the 'intention' that the sender has in sending the message (e.g. to inform, to request information, to delegate goals), and the actual message content.

```java
+!leave(home) //agent comes to have a new goal of leaving home
	: not raining & not ~raining // in a circumstance such that it is not sure whether it is raining or not
	<- !location(window); //a goal of being located at the window
		?curtain_type(Curains); // retrieve from the belief base the particular type of curtains that the agent is now capable of perceiving
		open(Curtains); // an action of opening the curtain 
		... .
		
+!leave(home)
	: not raining & not ~raining
	<- .send(mum, askIf, raining); //send message to mum 
		... .
```

#### Expression

* Each formula in the context and body of a plan must have a **boolean** value (including internal actions). For example, $'X > Y * 2'$, the plan is only applicable if this condition is satisfied. Note that, in a plan body, the plan will fail, in a similar way to when an action fails, if the condition is not satisfied.
* `==` and `\==` are used for 'equal' and 'different', respectively — two terms meed to be exactly the same for `==` to succeed.
* `=` operator is used to check whether two terms are **unified**.
* `=..` which is used to deconstruct a literal into a list. For example,  $p(b,c)[a1,a2] =.. [p, [b,c], [a1,a2]]$.



#### Plan labels

We can think the label as a name of plan. In fact, all plans have labels automatically generated by ***Jason*** , if we do not give a specific name for each plan.

The label itself does not have to be a simple term such as 'label', it can be have the same form of a predicate. That is, it can include annotations too.



## Example 

A domestic robot has the goal of serving beer to its owner. Its mission is quite simple, it just receives some beer requests from the owner, goes to the fridge, takes out a bottle of beer, and brings it back to the owner. However, the robot should also be concerned with the beer stock (and eventually order more beer using the supermarket’s home delivery service) and some rules hard-wired into the robot by the Department of Health (in this example this rule defines the limit of daily beer consumption).*

In this system, there are three agents: robot, owner and the supermarket. And the possible perceptions that the agents have are :

* `at (robot, Place)`: to simplify the example, only two places are perceived, `fridge `(when the robot is in front of the fridge) and `owner`(when the robot is next to the owner).
* `stock(beer, N)`: when the fridge is open, the robot will perceive how many beers are stored in the fridge(N).
* `has(owner, beer)`: is perceived by the robot and the owner then the owner has a (non-empty) bottle of beer.

Besides perception and action, the agents also send messages to each other. The owner send message to the robot when he wants another beer. The message type is 'achieve', so when the robot receives this message it will have `has(owner, beer)` as a new goal. A similar message is sent to the supermarket agent when the robot wants to order more beer. The supermarket sends a ‘tell’ message to robot when it has delivered the order. Tell messages are used to change the agent’s beliefs ( delivered(beer,N,OrderId) in this case).

### owner agent

The owner's initial goal is `get(beer)`. Thus  when it starts running the event `+!get(beer)` is generated and the plan labelled `@g` is triggered (most plans in the source are identified by a label prefixed by `'@'`). This plan simply sends a message to the robot asking it to achieve the goal `has(owner, beer)`.

When the robot has achieved the goal, the owner perceives this, and the belief `has(owner, beer)[source(percept)]` is added to the belief base. This change in the belief base generates an event that triggers plan `@h1`. When executing `@h1`, the subgoal `!drink(beer)`is created, and then the owner starts drinking, by recursively executing plan `@d1`. The event `+!drink(beer)` has two relevant plans (`@d1` and `@d2`) and they are selected  based on their context: `@d1` is selected whenever the owner has any beer left and `@d2`otherwise.

The beer will eventually be finished (the percept `has(owner, beer)` is removed), and the intention of drinking also finishes (the `!drink(beer)` plan recursion stops). The event `-has(owner, beer)` then triggers plan `@h2` the plan create a new subgoal (`!get(beer)`), which starts the owner behavior from the beginning again. 

The last plan simply prints a special type of message received from an agent identified in the source as Ag . Note that the message is removed from the belief base after the `.print` internal action is executed. This is necessary because, when such message arrives, an event is generated only in the case where that message is not already in the belief base.

### supermarket agent

It has an initial belief representing the last order number and a plan to achieve the goal of processing an order for some agent. The triggering event of this plan has three variables: the product name, the quantity and the source of the achievement goal (the sender of the achieve message in this case). When selected for execution, the plan consults the belief base for the last order number ( `?last_order_id(N)` ); increments this number ( `OrderId = N + 1 `); updates the belief base with this new last order number ( `-+last_order_id(OrderId) `); delivers the product (through an external action); and finally sends a message to the client (the robot, in this case) telling it that the product has been delivered.

### robot agent

The robot holds the initial belief that the beer is available in the fridge `available(beer, fridge)`, the robot only perceive the stock when the robot is in front of the fridge and it is opened, thus the robot need to remember what it saw  inside the fridge. Plans `@a2` and `@a3` update the availability belief according to the stock perception.

The belief has a rule that determines whether the owner already drank the maximum number of beers allowed each day. When the robot gives a beer to the owner, the belief `consumed` with current date will be added into the belief base,  the rule gets the current data using the internal action `.date`, then uses another internal action `.count` to count how many `consumed` beliefs there are in the belief base. `too_much` belief hold if the QtdB (quantity) is greater than the limit of beers.

The main task of the robot is to achieve `has(beer, owner)`, created by an achieve message received from the owner. There are three plans to achieve this(`@h1`, `@h2`, `@h3`). The first plan is selected when there is beer available and the owner has not drunk to much beer today. In the plan execution, the robot goes to the fridge (subgoal), opens it (external action), gets a beer (external action), closes the fridge (external fridge), goes back to the owner (subgoal) and finally gives the beer to the owner (external action). 

The second plan is selected when there is no more beer in the fridge, the robot will send an achieve message to supermarket to delivery five more beers. The `has(beer, owner)` should be resumed when beers are delivered, the plan `@a1` does that when the message from supermarket is received.

The third plan is received when the owner has drunk more than limited number of beers, the robot is not allowed to give  owner beers. 
