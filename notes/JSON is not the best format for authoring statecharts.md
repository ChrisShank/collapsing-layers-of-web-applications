# JSON is not the best format for authoring statecharts

> Is JSON really the best we can do as a DSL for building statecharts? [source](https://twitter.com/mpocock1/status/1439853560150298624)

My take on this question is that JSON *might* be the best format to interop statecharts between computers like interpreters and tooling, but it's a suboptimal format for authoring statecharts. Let's try to dig into why!

(Just to be clear the follow is less a critque of `xstate` and more an examination of how we can make authoring statecharts better!)

In a statechart there are numerous cases where one or more entities can exist. For example there can be one or more actions defined on a `entry`/`exit`/transition, there could be one or more targets on a transtion, there could one or more transitions per event, ect. So if you are designing a JSON API for statecharts, you come to a conondrum where you either make everything plural by default or you provide shorthands so each of these cases can be singular or plural. The former keeps the API significantly smaller and makes the implementation easier, but comes at the cost that the developer must [pluralize](https://www.swyx.io/preemptive-pluralization/) everything. In JSON this means that these things must be wrapped in a array even if there is only one action. This comes at the cost of burdening the developer with an API that is much less concise, especially when singular instances are the most common. One upside is that refactoring an existing statechart becomes a little easier since, for example, there is no need to wrap a single action in an array to add another one as shown below.

```diff
- entry: [“someAction”],
+ entry: [“someAction”, “someOtherAction”]
```

`xstate` chose to go down the later; add shorthands for all of these different cases. While I think this was *probably*
the better path of the two it does come with its own dowsides. Mainly the surface layer of the API becomes significantly larger and the implementation becomes more complex (to a certain degree). At least the developer can type less characters up front, but the cost happens when you need to refactor from a singular shorthand as shown below:

```diff
- entry: “someAction”,
+ entry: [“someAction”, “someOtherAction”]
```

This may seem like an unecessary critisism, but if I'm being honest this type of consistent micro-decisions really adds up and takes way from what we are trying to do: model with statecharts. It affects the learning curve since there is so much more syntax to learn (on top of the underlying concepts of statecharts). It affects cognitive load and descision fatigue because we need to consitently think about whether you might need to wrap something in an array in the future.

Similarly this disonnance reveals it self since objects in JSON are key-value pairs. State nodes and transtions are objects, but most of the time they have a single attribute (e.g. an event just has a `target`). So in these cases we end up in the same dilemma as above; should we allow a shorthand for these cases? Like above I think the shorthad makes the most sense, but the downsides still exist. In the example below, look at all of the refactoring you have to do to add one action to a transtion!

```diff
- on : { SOME_EVENT: 'someTarget' },
+ on : { SOME_EVENT: { target: 'someTarget', actions: 'someAction' },
```

Brackets, quotes, and colons, are syntax that gets in the way of what we are trying to express. They add significantly more lines, indents, and characters to our model and I suspect that a well-defined statechart DSL could reduce these quantities by up to 30-60%. Furthermore this extra syntax starts to weaken the 1:1 mapping between our code and the visual state chart we are trying to code.

If we want to make statecharts easier to write/read/refactor and if we want to lower the learning curve, and the cognitive load developers currently experience then I suspect that figuring out a new language to express ststecharts might be a good starting point to address all of these problems!
