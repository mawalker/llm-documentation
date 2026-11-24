# Prompting
- What makes a good prompt
- Common patterns: instructions, few-shot, role framing, output formatting
- Iterating and debugging prompts
- Chaining and decomposition
- Anti-patterns
- Prompting reasoning models: give the goal and constraints, not step-by-step reasoning instructions (see [Core Concepts](../core-concepts/core-concepts.md))

## Example: when the model says it did the right thing but didn't

An example of LLM not understanding, doing the wrong thing, and then telling you it did the right thing. When you notice this, simplify your instructions and make them smaller and simpler.

```
what you did was not helpful, you added double quotes (") around the start and end of folders and files, remove the double quotes and move the files you renamed back to the folders with the name without the double quotes. then delete the folders with the double quotes. Then update the linkes to follow this pattern `[Local File](<./My Folder/Document Name.md>)` with opening < and closing > when they contain a path including any spaces.

```

![opencode session with a local Qwen model: the model reports that links use angle brackets and escaped spaces, while the directory tree still shows folders wrapped in double quotes](prompt-failure-screenshot-example-1.png)

What went wrong in the screenshot (opencode running `qwen3-coder:30b` locally through Ollama):
- The model reports the folders are fixed, but the `tree` output still shows folders with double quotes in their names.
- It also claims it "properly escaped spaces with backslashes" in links (`Core\ Concepts`). That isn't valid Markdown: CommonMark backslash escapes only work on ASCII punctuation, not spaces. Link paths with spaces need the `<...>` form instead.
- Lesson: check the actual result (here, the file tree), not the model's summary. The first prompt packed several steps into one message; the follow-up below asks for a single step.

The shorter follow-up prompt:

```
move the files that start with " to be in the folder with the same name as each file without "(s) in it 
```