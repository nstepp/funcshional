# funcshional
Functional style shell commands

Command | Description
------- | -----------
**foldl** | Accumulate results over lines
**foldl1** | Like foldl, but initialize with first line
**ix** | Pick nth whitespace-delimited text out of each line
**map** | Run a command on each line
**mapDir** | Run a command in each directory

## foldl

    foldl <cmd> <init>

Runs `cmd` with variables `$x` and `$y` available for expansion, with accumulation in `x`, starting with `init`.

```
nstepp $ ls -l
total 56
-rw-r--r--  1 nstepp  nstepp  1068 Dec  7 12:08 LICENSE
-rw-r--r--  1 nstepp  nstepp    46 Dec  7 12:08 README.md
-rwxr-xr-x  1 nstepp  nstepp   182 Dec  7 11:12 foldl
-rwxr-xr-x  1 nstepp  nstepp   128 Dec  7 11:12 foldl1
-rwxr-xr-x  1 nstepp  nstepp   108 Dec  7 11:13 ix
-rwxr-xr-x  1 nstepp  nstepp   138 Dec  7 11:13 map
-rwxr-xr-x  1 nstepp  nstepp   205 Dec  7 11:13 mapDir
nstepp $ ls -l | ix 6 | foldl 'echo $((x+y))' 0
1875
nstepp $
```

## foldl1

    foldl1 <cmd>

Runs `cmd` with variables `$x` and `$y` available for expansion, with accumulation in `x`, starting with the first available line.

```
nstepp $ ls -l
total 56
-rw-r--r--  1 nstepp  nstepp  1068 Dec  7 12:08 LICENSE
-rw-r--r--  1 nstepp  nstepp    46 Dec  7 12:08 README.md
-rwxr-xr-x  1 nstepp  nstepp   182 Dec  7 11:12 foldl
-rwxr-xr-x  1 nstepp  nstepp   128 Dec  7 11:12 foldl1
-rwxr-xr-x  1 nstepp  nstepp   108 Dec  7 11:13 ix
-rwxr-xr-x  1 nstepp  nstepp   138 Dec  7 11:13 map
-rwxr-xr-x  1 nstepp  nstepp   205 Dec  7 11:13 mapDir
nstepp $ ls -l | ix 6 | foldl1 'echo $((x+y))'
1875
nstepp $
```

## ix

    ix <num>

Picks the `num`th word. Equivalent to `awk '{ print $num; }'`.

```
nstepp $ ls -l
total 56
-rw-r--r--  1 nstepp  nstepp  1068 Dec  7 12:08 LICENSE
-rw-r--r--  1 nstepp  nstepp    46 Dec  7 12:08 README.md
-rwxr-xr-x  1 nstepp  nstepp   182 Dec  7 11:12 foldl
-rwxr-xr-x  1 nstepp  nstepp   128 Dec  7 11:12 foldl1
-rwxr-xr-x  1 nstepp  nstepp   108 Dec  7 11:13 ix
-rwxr-xr-x  1 nstepp  nstepp   138 Dec  7 11:13 map
-rwxr-xr-x  1 nstepp  nstepp   205 Dec  7 11:13 mapDir
nstepp $ ls -l | ix 6
1068
46
182
128
108
138
205
nstepp $
```

## map

    map <cmd>

Runs a command on each line (like a simplified `xargs`)

```
nstepp $ find .. -name '*.md'
../funcshional/README.md
nstepp $ find .. -name '*.md' | map basename
README.md
nstepp $
```

## mapDir

    mapDir <cmd>

Runs a command in each directory specified by input lines.

```
nstepp $ find .. -name '*.md'
../funcshional/README.md
nstepp $ find .. -name '.git' | map dirname | mapDir "git status" 
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working tree clean
nstepp $
```
