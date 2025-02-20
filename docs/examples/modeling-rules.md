# Modeling with Rules

## Rewriting Deontic Positions in Terms of Power

Let's imagine that we have a [duty](../reference/positions.md#deontic-frames) `d` for `john` to `#pay` another [agent](../reference/objects-and-events.md#agents) `paul`.

```
duty {
	holder: john
	counterparty: paul
    action: #pay
    violation: timeout
} as d
```

Now we have obtained a deontic view on the payment. However, this only models part of the problem. We could introduce a [transformational rule](../reference/rules.md#transformational-rules) that refines the context when a payment [duty](../reference/positions.md#deontic-frames) is introduced.

```
d -> {
	john.#pay => +power {
		holder: paul
        action: #declare_fulfillment { item: d }
        consequence: +d.fulfilled
    }
    timeout => +power {
        holder: paul
        action: #declare_violation { item: d }
        consequence: +d.violated
    }
}
```

Within this context, the [agent](../reference/objects-and-events.md#agents) `john` can perform the [action](../reference/objects-and-events.md#action-events) `#pay`, which allows the counterparty `paul` to `#declare_fulfillment` of the original [duty](../reference/positions.md#deontic-frames).

Moreover, we can provide further detail on the violation mechanism. In this case, we actively provide the counterparty with the [power](../reference/positions.md#power-frames) to `#declare_violation`.

This explicit model allows for better tracing of the activities performed by the [agents](../reference/objects-and-events.md#agents) and brings [power](../reference/positions.md#power-frames) at the forefront of the modeling strategy.

## Rewriting Knowledge in Terms of Power

Transformational rules can be seen not only as _epistemic_ [duties](../reference/positions.md#deontic-frames) of producing knowledge, but also as [powers](../reference/positions.md#power-frames).

For instance, we could model the epistemic knowledge that a `car` constitutes a `vehicle`.

```
car -> vehicle
```

However, thanks to [transformational rules](../reference/rules.md#transformational-rules) being able to create contexts, we can further refine the implications of some piece of knowledge.

For example, when modeling a Vehicle Licensing Agency system, we might want to express [duties](../reference/positions.md#deontic-frames) and [powers](../reference/positions.md#power-frames) related to the existence of vehicles.

```
car -> {
    duty {
        holder: *
        action: +vehicle
    }
    power {
        holder: *
        action: #state { item: vehicle }
        consequence: +vehicle
    }
}
```

Here we express the _epistemic_ [duty](../reference/positions.md#deontic-frames) to activate the `vehicle` state, but also the [power](../reference/positions.md#power-frames) for a someone to actively `#state` the existence of a `vehicle`.
