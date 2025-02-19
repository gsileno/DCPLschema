# Objects & Events

At a fundamental level, DCPL relies on the common-sensical distinction of **objects** (-types) from **events** (-types), an ontological ground confirmed e.g. in LKIF-core[^1].

[^1]: Hoekstra, R., Breuker, J., Di Bello, M., Boer, A., et al.: The LKIF core ontology of basic legal concepts. LOAIT 321, 43–63 (2007)

## Objects

Objects are persistent data within a DCPL program instance and they are used to represent [agents](#agents) and facts about the state of the program.
Everything that doesn’t actively change the system is an object.
All objects (including states) are denoted as literals (e.g. `user`).
Objects can be active or inactive, allowing them to also be used as boolean expressions in [rule conditions](rules.md#transformational-rules).

### Descriptors

Extensional semantics applied in formal languages are based on set-theory and consider
classes or types as sets of instances.
This means that they (theoretically) maintain access to an abstract data structure, used e.g. to enlist all instances, and then to check whether an instance belongs to it.
Rather than sets of objects centrally maintained, DCPL considers the association of several descriptors to each object (e.g. like tags). In programming terms, one can think of this as _composition_ over _inheritance_.

<figure markdown="span">
    ![Descriptors](../assets/images/descriptors.svg)
</figure>

As an analogy, consider how people generally keep in their own wallets id-cards, library cards, discount coupons, membership cards, etc.
These are examples of several “identity” certificates (unique or not) operational in different contexts, which have been issued by an authority, and of which no other copy may exist, except the one people keep with them.

DCPL aims to bring the qualification act in the foreground, and it does so by means of [qualification events](#qualification-events) and the `is` operator. For instance, we can check whether `john` is to be considered a `student`:

```
john is student
```

#### Primitive Types

Supported primitive types are all descriptors, and their instances are automatically described with them:

- `string` (e.g., `"example" is string`)
- `number` (e.g., `42 is number`, `3.14 is number`)
- `boolean` (i.e., `true is boolean` and `false is boolean`)
- `undefined` (i.e., `undefined is undefined` )

A primitive type instance (e.g. `42` or `"urn:user:001"`) also acts as a descriptor which represents the identity function for that value.

#### Object Properties

Each object and each of its properties naturally act as a descriptor.

!!! example

    A `dataset` A is considered to be owned by some `organization` X:

    ```
    "urn:org:x" becomes organization
    "urn:dataset:a" becomes organization.dataset
    ```

#### Set Operations

Unions (using the OR operator `|`) and intersections (using the AND operator `&`) of descriptors are also descriptors.

!!! example

    Any agent which is either `student` or `staff` is also a `university_member`:

    ```
    student | staff becomes university_member
    ```

A union descriptor can be expected to have only properties that are shared among all the descriptors. An intersection descriptor instead can be expected to have all the properties combined.

### Agents

An object becomes an agent when it is granted the ability to perform one or more actions (as discussed in [normative positions](positions.md)).
Agents will often act as specifiers, indicating the type or group of individuals that a frame attribute applies to.

A special type of agent is `*`, which represents all agents.

!!! example

    If a [duty](positions.md#deontic-positions) is held by the agent `teacher`, every agent that has the `teacher` descriptor has that [duty](positions.md#deontic-positions).

## Events

Events actively change the system (i.e. they represent a transition) and they object they refer to.

### Action Events

Actions are primitive events denoted as objects prefixed with `#` (e.g. `#shout`). An action can be performed to trigger a [reactive rule](rules.md#reactive-rules) which in turn mutates the system. However, the object `shout` only changes its state instantaneously when the action event is fired.

### Production Events

A special case of transition events are those concerning the (de-)activation of objects. For instance, the activation of a raining state is denoted with `+raining`, whereas its disactivation as `-raining`.

!!! tip

    When transient, events should rather be modeled as objects (e.g. `borrowing` or `raining`), to denote for their ongoing state.

### Qualification Events

To assign a [descriptor](#descriptors) to an object, we can use the formula:

```
object becomes descriptor
```

To remove a [descriptor](#descriptors) from an object, we can use the formula:

```
object leaves descriptor
```

## Refinements

Objects and events can have internal properties that can be specified or refined via the program. For instance, an [action](positions.md#power-frames) could be specified further referring to the thematic roles of verbs used in linguistics (agent, patient, recipient, instrument, etc.).

For the description of internal components, DCPL follows an object-access syntax. Possible internal properties may be accessed as in OOP (e.g. `user.online`).

!!! example

    This is how we model that `person` objects should have a `spouse` property which is also a `person`:

        person {
            spouse: person
        }

However, to reify the informational model, certain objects (e.g. the [normative positions](positions.md)) come with fixed parameters.
