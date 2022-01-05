# How to use sed

String substitution can be achieved using the UNIX tool, Stream EDitor (sed), is fast and scalable for this purpose.

## Shell 

The shell you're using will impact sed's behavior. Therefore I've broken below guidance down by OS.

## String substitution with sed (Linux/UNIX)

A string pattern in a file on Linux/UNIX can be replaced using this format:
`sed -i 's/old-text/new-text/g' input.txt`

The `s` tells sed to use its substitute command, which will replace whatever you put in `old-text` with whatever you put in `new-text` fields.

This command directly edits whatever file you specify in `input.txt` field.

## String substitution with sed (macOS)

A string pattern in a file on macOS can be replaced using this format:
`sed -i '' 's/old-text/new-text/g' /path/to/input.txt`
