# `english-script`

Embed natural language in your code.

## Usage

`english-script` expects your OpenAI key to be in an environment variable `openai_key`.

You can then use it like so:

```js
const f = await makeFunction({
  description: "determine if prime",
  testCases: [
    [1, false], // By definition
    [2, true],
    [4, false],
  ],
});
[53, 44].map(f); // Gives back [true, false]
```

## Implementation details

1. Caching (https://github.com/uriva/rmmbr) is used to avoid repetitive slow/expensive calls to OpenAI.
1. The generated code is checked for side effects (e.g. network calls or external dependencies), to make sure it is safe to run.
1. The generated code is tested against your provided test cases.

## So what is this?

### Background

We can now generate code with fairly good quality, depending on the task, using LLMs.

The common developer goes through this cycle:

1.  Prompt LLM to get some code to do some task
1.  Copy the code into their IDE
1.  Read it to make sure it makes sense
1.  Write some tests and running them to make sure it works as expected
1.  Commit the code into the repo

### Argument

Committing code generated by a machine doesn't feel right because it's kind of like committing a compiled binary.

If I compiled some code and saved the binaries, I now have in my repo two things which are coupled together, and it's unclear if the versions match, how they match, which derives from which, and therefore it becomes unclear which I need to iterate on in order to improve.

This is why it's a common practice to commit only the "topmost" work, i.e. the one others derive from. This has always been high level code, until now. We've now reached the point where at least in some cases the topmost form is a prompt in natural language description, and so we should commit that form instead.

Furthermore most of what LLMs generating code are really good at is code that already exists in some form in the web, in the form of libraries or code examples in stackoverflow. So it's unlikely that adding generated code into our repo will contribute any concrete innovation, rather than provide more duplication.

### Problem

How do we commit natural language alongside code and iterate on it, so that we can avoid duplication, benefit from the LLMs incredible abilities and have a workflow that makes sense?

### Solution

`english-script` is a js library that allows you to embed natural language descriptions of pure functions inside your code seamlessly. It calls an LLM API in the background to get code and replace it on the fly.
