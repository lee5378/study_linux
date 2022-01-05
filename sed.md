# How to use sed

String substitution can be achieved using the UNIX tool, Stream EDitor (sed), which is fast and scalable for this purpose.

## Shell 

The shell you're using will impact sed's behavior. Therefore I've broken below guidance down by OS.

## String substitution with sed 

### Linux/UNIX

A string pattern in a file on Linux/UNIX can be replaced using this format:
`sed -i 's/old-text/new-text/g' input.txt`

### macOS

A string pattern in a file on macOS can be replaced using this format:
`sed -i '' 's/old-text/new-text/g' /path/to/input.txt`

### General notes

The `s` tells sed to use its substitute command, which will replace whatever you put in `old-text` with whatever you put in `new-text` fields.

This command directly edits whatever file you specify in `input.txt` field. If you want to output to a new file, add `> output.txt` at the end, e.g. `sed -i 's/old-text/new-text/g' input.txt > output.txt`.

Verify changes made using `cat` against the filename, e.g. `cat intput.txt`.

### Example in macOS Terminal

```
david@macbook Downloads % touch demo.txt
david@macbook Downloads % echo "I will not eat my vegetables" >> demo.txt
david@macbook Downloads % cat demo.txt
I will not eat my vegetables
david@macbook Downloads % sed -i '' 's/not //g' demo.txt
david@macbook Downloads % cat demo.txt
I will eat my vegetables
```
