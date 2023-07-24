# Bash Scripting

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Bash Scripting](#bash-scripting)
  - [Interactive Script](#interactive-script)
  - [Execute Commands in Script](#execute-commands-in-script)
  - [Math in terminal](#math-in-terminal)
  - [Conditionals](#conditionals)

<!-- /code_chunk_output -->

Check current shell in use:

```bash
$ which $SHELL
```

Execute a bash script file:

```bash
$ bash file_name.sh
```

How to hang up bash:

```bash
$ sleep 3
```

Make a bash file executable:

```bash
$ chmod +x file_name.sh
$ ./file_name.sh
```

Local Variables:

```bash
name="Reza"
echo "Hello There $name"
```


## Interactive Script

How to get input from user:

```bash
echo "what is your name?"
read name
echo "Hello $name"
```

How to get arguments:

```bash
# in script file
name=$1

# in command line
$ ./script.sh Reza
```

## Execute Commands in Script

```bash
user=$(whoami)
```

**NOTE:** Self-made variables cannot be used by child processes. We use `export` command to make it available globally. It's **NOT** permanent yet.

**NOTE:** We use `~/.bashrc` file to export variables permanently.

## Math in terminal

```bash
$ echo $(( 2 + 3 ))
```

## Conditionals
