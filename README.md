# ArrayLang-2
A tool for generating array-based expressions from a Python-like script
## What is this used for?
This tool is intended for easy creation of array-based expression in the [Automate](https://play.google.com/store/apps/details?id=com.llamalab.automate) app.
## What is an array-based expressions
Let's consider a program that takes in an input n and calculates the n-th fibonacci number.
```python
input(n)
a = 0
b = 1
c = 1
i = 0
n = n - 1
while i < n:
    c = a + b
    a = b
    b = c
    i += 1
print(c)
```
Implementing the flow in the app would require placing one [variable set](https://llamalab.com/automate/doc/block/variable_assign.html) blocks for each assignment done, resulting in 9 blocks.

Rather than assigning one variable at a time, we could create an array to assign multiple variables at once.

We could transform the program into something like this:
```python
input(n)
arr = [0, 1, 1, 0, n - 1]
while arr[3] < arr[4]:
    arr = [arr[1], arr[1] + arr[0], arr[1] + arr[0], arr[3] + 1, arr[4]]
print(arr[2])
```
So now there are only 2 assignments, resulting in 2 blocks.
## Why reduce block count?
One of the main reasons to reduce the number of blocks in a flow is to keep your block count below 30 in your flow.
Automate only allows users without premium to run up to 30 blocks at one time, whether it's one flow with 30 blocks or 10 flows with 3 blocks each.
By keeping the block count less than 30, you're making the flow accessible for non-premium users.
## How to use it
### Running the tool
First, clone or download the ZIP.
After downloading it, navigate to the repo folder and open index.html with your preferred browser.
That's it.

Alternatively, I also put up a [webpage](https://realofficialturf.github.io/ArrayLang2/index.html) that you can immediately access to run the tool on your browser.
### Writing the script
The first line of the script has to start with the "array" keyword, followed by the array name and the number of elements of the array, like so.
```
array g[4]
```
You can then write assignments to array elements like these:
```
g[2]=g[0]+g[1]
g[0]=g[1]
g[1]=g[2]
g[3]=g[3]+1
```
To improve readability, the `##DEFINE` tag can be used to replace the array elements with a name.
```
##DEFINE a g[0]
##DEFINE b g[1]
##DEFINE c g[2]
##DEFINE i g[3]
##DEFINE n g[4]
```
*Note that the ##DEFINE tag does **not** discriminate and also replaces values in strings (and even keywords - don't overwrite the if/else/elif keyword!)*

Taking these together, a simple script is made
```
array g[4]
##DEFINE a g[0]
##DEFINE b g[1]
##DEFINE c g[2]
##DEFINE i g[3]
##DEFINE n g[4]
c=a+b
a=b
b=c
i=i+1
```
This script is turned to an array...
```
[g[1],(g[0]+g[1]),g[0]+g[1],g[3]+1]
```
...And the array can be used directly on the [variable set](https://llamalab.com/automate/doc/block/variable_assign.html) block!
### Example
Here is an example script that processes the player action and initiates a potion sale when the player decides to buy a potion. If the player has sufficient money, the potion is bought and the gold is taken away from the player. Additionally, a text is updated to reflect the outcome of the player action.
```
array g[4]
##DEFINE goldmoney g[0]
##DEFINE potionitem g[1]
##DEFINE playeraction g[2]
##DEFINE actionresult g[3]
#Comments can be placed on its own in unindented code
if playeraction="buy potion":
	if gold>100:
		goldmoney=goldmoney-100
		potionitem=potionitem+1 #Comments can only be placed inline when there is an indentation
		actionresult="Potion bought!"
	else:
		actionresult="Not enough gold!"
else:
	actionresult="Nothing to do."
```