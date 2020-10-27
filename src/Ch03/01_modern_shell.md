# modern shell

## Better Alternative

### find -> fd

```
{}: A placeholder token that will be replaced with the path of the search result (documents/images/party.jpg).
{.}: Like {}, but without the file extension (documents/images/party).
{/}: A placeholder that will be replaced by the basename of the search result (party.jpg).
{//}: Uses the parent of the discovered path (documents/images).
{/.}: Uses the basename, with the extension removed (party).
```

```
# Convert all jpg files to png files:
fd -e jpg -x convert {} {.}.png

# Unpack all zip files (if no placeholder is given, the path is appended):
fd -e zip -x unzip

# Convert all flac files into opus files:
fd -e flac -x ffmpeg -i {} -c:a libopus {.}.opus

# Count the number of lines in Rust files (the command template can be terminated with ';'):
fd -x wc -l \; -e rs
```

### grep -> ripgrep

## Dev

### FZF

### Autojump

## Tools

### mycli

### pgcli

### convert

### k9s

### tokei

### git
