# Bash Scripting

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Bash Scripting](#bash-scripting)
  - [Interactive Script](#interactive-script)
  - [Execute Commands in Script](#execute-commands-in-script)
  - [Math in terminal](#math-in-terminal)
  - [Conditionals](#conditionals)
  - [Case Statements](#case-statements)
  - [Loops](#loops)
    - [While](#while)
    - [Until](#until)
    - [For](#for)
      - [Break & Continue](#break--continue)
        - [Break](#break)
        - [Continue](#continue)

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

```bash
if [[ $coffee == "y" || $tarnished == $beast ]]; then
  echo "You're Awesome!"
else
  echo "Leave right now!"
  exit 1
fi
```

**NOTE:** `||` is or, `&&` is and.
**NOTE:** We can nest `if` statements.

```bash
if [[ $coffee == "y" ]]; then
  echo "You are Awesome!"
elif [[ $coffee == "n" ]]; then
  echo "Get out!"
else
  echo "Wrong Answer."
fi
```

## Case Statements

```bash
echo "Enter your class:
1- Samurai
2- Prisoner
3- Prophet"

read class

case $class in
  1)
    type="Samurai"
    hp=10
    ;;

  2)
    type="Prisoner"
    hp=20
    ;;
esac
```

## Loops

### While

```bash
x=1

while [[ $x -le 100 ]]
do
  read -p "Pushup $x: Press enter to continue"
  (( x ++ ))
done
```

reading files:

```bash
while read -r line; do
echo "Line $x $line"
(( x ++ ))
done < hamlet
```

**NOTE:** `while true` goes on forever.

### Until

``` bash
until [[ $order == "coffee" ]]
do
  echo "Would you like coffee or tea?"
  read order
done
echo "Excellent choice, here is your coffee."
```

### For

```bash
for cups in {1..10};
do
  echo "You've had $cups cups of coffee today."
done
```

Checking Domains:

```bash
for x in google.com bing.com facebook.com;
do
  if ping -q -c 2 -W 1 $x > /dev/null; then
    echo "$x is up"
  else
    echo "$x is down"
  fi
done
```

**NOTE:** `-q` is quiet, `-c` is count if ICMP packets, `-W` is wait time.

```bash
for x in $(cat cities.txt);
do
  weather=$(curl -s http://wttr.in/$x?format=3)
  echo "The weather for $weather"
done
```

#### Break & Continue

##### Break

```bash
#!/bin/bash

echo "What do you want to check?"
read target

while true;
do
  if ping -q -c 2 -w 1 $target > /dev/null; then
    echo "Hey, You're up!!"
    break
  else
    echo "$target is currently down."
  fi
sleep 2
done
```

##### Continue

```bash
for x in {1..17};
do
  if [[ $x == 13 ]]; then
    continue
  fi
  echo "Floor $x"
  sleep 1
done
```