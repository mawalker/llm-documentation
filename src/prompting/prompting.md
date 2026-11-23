## Prompting
- What makes a good prompt
- Common patterns: instructions, few-shot, role framing, output formatting
- Iterating and debugging prompts
- Chaining and decomposition
- Anti-patterns


An example of LLM not understanding, doing the wrong thing, and then telling you it did the right thing. When you notice this, simplify your instructions and make them smaller and simpler.

```
what you did was not helpful, you added double quotes (") around the start and end of folders and files, remove the double quotes and move the files you renamed back to the folders with the name without the double quotes. then delete the folders with the double quotes. Then update the linkes to follow this pattern `[Local File](<./My Folder/Document Name.md>)` with opening < and closing > when they contain a path including any spaces.

```

![](prompt-failure-screenshot-example-1.png)

```
move the files that start with " to be in the folder with the same name as each file without "(s) in it 
```