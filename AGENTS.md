# GUIDELINES
This document defines the coding standards that agents should follow in this project. 

## Variable Names Should Communicate Intent

Choose variable names that make the value's purpose clear without requiring the reader to trace how it is used. Avoid vague abbreviations, generic placeholders, and names that describe only the value's type.

Names should be specific enough to distinguish the variable from similar values in the same scope. Short conventional names such as `i` may still be used when their meaning is obvious in a small, familiar context.

### Bad

```ts
const d = new Date();
const data = users.filter((u) => u.active);
const n = data.length;
```

The names do not reveal what the date represents, what kind of data was selected, or what is being counted.

### Good

```ts
const subscriptionRenewalDate = new Date();
const activeUsers = users.filter((user) => user.active);
const activeUserCount = activeUsers.length;
```

The names communicate their roles directly and reduce the need for comments or additional context.

## Comments Should Explain Why, Not What

Do not add comments that merely restate readable code. Use comments when the reasoning, constraint, tradeoff, workaround, or otherwise non-obvious behavior cannot be expressed clearly through the code itself.

Keep comments concise and current. A developer should be able to understand the important context quickly. If the code is difficult to understand because of unclear structure or naming, improve the code before relying on a comment.

### Bad

```ts
// Loop through every user.
for (const user of users) {
  // Check whether the user is active.
  if (user.active) {
    // Add the user to the active users array.
    activeUsers.push(user);
  }
}
```

These comments repeat exactly what the code already says and create extra text that must be maintained.

### Good

```ts
// Suspended accounts remain billable until the current cycle ends.
const billableUsers = users.filter(
  (user) => user.active || user.suspensionDate > billingCycleEndDate,
);
```

The comment preserves a business rule that is not obvious from the condition alone.

### Good: Preserve a Non-Obvious Technical Constraint

```ts
// Keep requests sequential to stay below the provider's per-account rate limit.
for (const accountId of accountIds) {
  await syncAccount(accountId);
}
```

The comment explains why an apparently faster parallel implementation was intentionally avoided.

## Prefer Meaningful Reuse Without Unnecessary Abstraction

Before writing new logic, look for an existing function, utility, component, or
module that already provides the required behavior. Reuse or extend it when doing
so preserves a clear API and does not create unrelated coupling.

When the same behavior or business rule is implemented in two or more places,
consider extracting it into one clearly named shared function. Prefer a shared
implementation when the copies must remain synchronized as the code changes.

Do not combine code merely because it looks similar. Keep implementations
separate when they represent different concepts, are likely to evolve
independently, or would require flags and conditionals that make the shared
abstraction harder to understand.

Avoid trivial single-use helper functions that only rename a short, readable
operation. Inline them into their sole caller when doing so makes the execution
flow easier to follow.

A single-use helper may remain when it provides a meaningful boundary, such as:

- naming an important domain concept
- isolating complex logic or side effects
- containing error handling or resource cleanup
- reducing the cognitive load of a long function
- satisfying a framework callback or interface
- enabling focused testing of behavior that is otherwise difficult to test

When modifying existing code, check the nearby scope for relevant duplication
and unnecessary one-use helpers. Keep refactoring proportional to the requested
change and do not expand into unrelated cleanup.

## Code Understanding Quiz

After completing any task that writes or modifies code, do not end with only a
summary. Give me an interactive quiz that verifies I understand the
implementation.

Before the quiz, provide only a brief orientation:

- what changed
- which files contain the important logic
- where I should begin reading the code

Then ask no more than five questions. Use fewer questions for trivial changes.

The questions should test genuine understanding rather than syntax trivia. When
relevant, cover:

1. The purpose of the change and the problem it solves.
2. The execution flow through the modified code.
3. Why an important implementation decision was made.
4. A likely edge case, failure mode, or debugging scenario.
5. How I could safely extend or modify the implementation myself.

Do not provide the answers immediately. Wait for me to answer the quiz.

After I respond:

- grade each answer individually
- clearly explain anything I misunderstood
- use hints before revealing an answer when I am close
- point me to the relevant file and code when explaining
- ask a revised follow-up question for any concept I did not understand
- finish with one small practice change I could implement myself
- do not implement the practice change unless I ask

Adjust the difficulty to the complexity of the task and my demonstrated
knowledge. Focus especially on concepts that transfer to future programming
work.

A coding task is not educationally complete until I have attempted the quiz or
explicitly chosen to skip it.
