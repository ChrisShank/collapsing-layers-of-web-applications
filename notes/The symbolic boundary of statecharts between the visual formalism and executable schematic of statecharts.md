# The symbolic boundary of statecharts; hidden between the visual formalism and executable schematic

> "Design is about deciding on the boundaries." – [Grady Booch](https://twitter.com/Grady_Booch/status/1444754474426191873)

The above frames a tension that I think deeply perpetuates the design of most statechart tooling and something that has been on my mind lately as I work on [StateML](https://twitter.com/StateML_org). That tension stems from duality that a statechart exists as; it is both a [visual formalism](https://link.springer.com/referenceworkentry/10.1007%2F978-0-387-39940-9_444) and a executable schematic. The former provides a way reason and model behavior defined in a mathematically rigorous manner. The later allows you to take that model and bring it to life by running it as actual code. This duality can also be described as the declarative and imperative parts of the statechart. The declarative part of the statechart is the visual formalism hiding in a textual representation. The imperative part enable the statechart to become executable through a combination of a statechart interpreter + the user-defined actions/guards/activites written in an imperative programming language. Without its imperative part the statechart is just a diagram, and without its declarative part the statechart is not much more than imperative code. The two are inseperable, 


So the declarative and imperative are symbolically bound together.

It's unclear of the implications, but XState blurs this symbolic boundry with action creators and inlined actions/guards/services. These implementations that are inlined in the statechart definition cannot be visualized. 
