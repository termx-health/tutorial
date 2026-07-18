TermX includes built-in **task management** to coordinate the editorial work around terminology. Tasks connect a unit of work — a review, an approval, a translation proposal, a concept-linkage request — to the resource it concerns, so the work is tracked in context rather than in a separate tool.

![Task list](files/tutorial/tasks.png)

## Where tasks appear

- On a resource **summary** (code system, value set, concept map, …) a card lists the tasks connected to that resource. By default only active tasks are shown; *All tasks* reveals the full history.
- In the [SNOMED CT browser](page:snomed-ct-browser), each proposed translation raises a **concept review** task; its number and status are shown next to the concept, and context links let you jump between the task and the browser.

## Task flow

A task carries a status that drives what happens next. For example, a SNOMED translation task that is **accepted** causes the proposed designations to be written to the connected SNOMED branch; in other cases the proposal is kept in history but not applied.

## Visibility

Since release 3.1, task visibility is **role-based**: view-only users do not see the task list, editors see their own and assigned work, and publishers see the broader set of tasks for the resources they publish. See [permissions](page:permissions) for how roles are configured.
