# Composite Objects

More complex institutional concepts (e.g., ownership) can be described as a
composition of other [directives](directives.md). DCPL allows for combining the objects above into reusable, parameterizable compounds. All [directives](directives.md) inside a composite object are defined as long as the object is active.

## Base Syntax

Composite objects have a function-like syntax, as they can receive any amount of parameters that are scoped to the object definition inside.

=== "Code"

    ```
    ownership(person, item) {
        ...
    }
    ```

=== "JSON"

    ```json
    {
        "compound": "ownership",
        "args": {
            "person": "person",
            "item": "item"
        },
        "content": []
    }
    ```

## Parameter Descriptors

For composite objects, parameters names act as [descriptors](objects-and-events.md#descriptors) by default, scoping the use of a composite object. However, the user can alias the internal reference to change its scoped reference name or to distinguish between the same [descriptors](objects-and-events.md#descriptors).

=== "Code"

    ```
    under_examination(teacher: person, student: person) {
        ...
    }
    ```

=== "JSON"

    ```json
    {
        "compound": "under_examination",
        "args": {
            "teacher": "person",
            "student": "person"
        },
        "content": []
    }
    ```

## Examples

If there is nothing else than [deontic](positions.md#deontic-frames) and [power frames](positions.md#power-frames) in compounds, the institutional/non-institutional knowledge decomposition is evident. We could in principle use any other means (ontologies, logic programs, etc.) for the domain and/or terminological knowledge.

!!! example

    We want to define a composite object that allows to model how `user` [agents](objects-and-events.md#agents) can block each other which deactivates their liberty to send and receive chat messages in both directions.

    ```
    blocking(subject: user, blocked: user) {
        -liberty {
            holder: blocked,
            action: #send_chat {
                recipient: subject,
                message: string
            }
        }
        -liberty {
            holder: subject,
            action: #send_chat {
                recipient: blocked,
                message: string
            }
        }
    }
    ```

However, with the introduction of [rules](rules.md) that are only evaluated while the object is active, we obtain a unique level of expressiveness.

!!! example

    Here we define the concept of _borrowing_ an item for a certain time:

    ```
    borrowing(lender: person, borrower: person, book, timeout: number) {
        power {
            holder: lender
            action: #request_return
            consequence: +duty {
                holder: borrower
                counterparty: lender
                action: #return { item: book }
            }
        }

        duty {
            holder: borrower
            counterparty: lender
            action: #return { item: book }
            violation: now() > timeout
        } as d1

        +d1.violation => +power {
            holder: lender
            action: #fine { recipient: borrower }
            consequence: +angry(borrower, lender)
        }
    }
    ```
