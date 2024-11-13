# libft - Your own library

I will present the most important function in the alphabetical order.  
Here the summary:

1. [ft_atoi](#ft_atoi)
2. [ft_bzero](#ft_bzero)
3. [ft_calloc](#ft_calloc)
4. [ft_itoa](#ft_itoa)
5. [ft_lstadd_back](#ft_lstadd_back)
6. [ft_lstadd_front](#ft_lstadd_front)
7. [ft_lstclear](#ft_lstclear)
8. [ft_lstdelone](#ft_lstdelone)
9. [ft_lstiter](#ft_lstiter)
10. [ft_lstmap](#ft_lstmap)
11. [Memory functions](#memory_functions)
12. [ft_memmove](#ft_memmove)

<a name="ft_atoi"></a>

## ft_atoi

The aim of this function is to convert a string `(char *)` into an `int`.  
I declare 3 `int`. An `i = 0` iterator, `res = 0` to store the final result and `sign = 1` which will determine the sign.
`sign = 1` because we multiply the sign with the `res` at the end. If we find a `-` during iterations the sign
will be invert.

```
if (str[i] == '-')
    sign = -1;
```

First we pass all the withespaces. It's not ask in the subject but the test of the francinette of asking this.
I don't know why...

```
while ((str[i]> = && str[i] <= 13) || str[i] == 32)
    i++;
```

Then, if a sign is find. If it is a plus so we just increment the `i`, if it's a minus so we have to invert the sign. Like said before.  
Then we have the main loop that will make the conversion. The condition of this loop are `str[i] != '\0'`, so will the string is not terminated.  
The other is `ft_isdigit(str[i])`, meaning that the actual char must be a number. Both conditions must be verified.

```
while (str[i] && ft_isdigit(str[i]))
    res = res * 10 + (str[i++] - '0');
```

So for the code in the loop, I'll directly with an exemple: our string is `"+321"`. We already handled the plus sign. `i = 1` and so we are on the `3` char.  
Firstly, our `res` equals to zero. We multiply it by ten `0 * 10 = 0`. This multiplication is here to make the space for the next digit. You'll understand.  
After that, we have `+ (str[i++] - '0');`. `str[i]` is equal to the char `3`. We need the ascii table for that.  
![ascii table image](https://media.geeksforgeeks.org/wp-content/uploads/20240304094301/ASCII-Table.png)

As we see, the letter `3` has for decimal `51`. In order to convert a char into an int, we need to use the decimal part. So, the letter `3` is equal to `51`.  
If we leave it here, `51` will be store in the int `res`. 51 is not equal to 3.  
To get the good result we can substract the letter `0` to the previous digit. The letter `0` is equal to decimal `48`. And so letter `3` minus letter `0`, is equal to decimal `51` minus decimal `48` -> result decimal `3`.
That's is the correct number. We can see it like this.  
`res = 0 * 10 + ('3' - '0')` the simple quote are here to tell you that is the column char that's being used.  
`res = 0 * 10 + (51 - 48)` for the decimal column. We can mix the two synthax : `res = res * 10 + (str[i] - 48);` is the same.  
That's for the loop, the `i++` is the lign is to reduce the number of line code. The iterator give to the loop its actual value and then increment itself.  
After the first incrementation, `res = 3` and the `res = 3 * 10 + (str[i] - '0')`, so you see now why the multiply by ten, in order to res be equal to 30 + the following digit (2). etc...

```
return (res * sign);
```

So we finally return the result multiply by 1 or -1, if it's or not a negative number in the initial string.

<a name="ft_bzero"></a>

## ft_bzero

A simple function that initialize a (void \*) with `'\0'` for each element of `s` for `n` elements.
The cast in a (char \*) is important because we want to put in each byte a NULL char. So we want to increment byte by byte.  
If we don't do the cast, the `void *` can be any types, for example a `int *`. If we increment on a `int *` we are going to jump by sizeof(int) so 4 bytes.

<a name="ft_calloc"></a>

## ft_calloc

We just malloc a string with a length `count` multiply by the size of a type `sizeof(int)` for exemple.  
If the malloc crashes, we return `NULL`. After that, we initialize every byte to `'\0'`.

<a name="ft_itoa"></a>

## ft_itoa

This function has for aim to convert an `int` into a `char *` (string).  
We have two functions in this file `int get_power(int n);` and of course `char *ft_itoa(int n);`.  
`get_power(int n)` has for aim to return the size of the malloc we need, if it's a negative the count will be incremented by one. The last loop count the power of the number `n`.
This power will tell us how long the `res` string has to be.  
After that, we call the `ft_calloc` function, if the initial `n` is negative we write a `'-'` at the first byte, we initialize our `i` to 1. This variable will be like a protection for our `'-'`
for not being overwrite by the last loop.  
`res[len_malloc] = '\0'` sets the end of the string.

```
while (len_malloc > i)
{
    len_malloc--;
    res[len_malloc] = nbr % 10 + '0';
    nbr /= 10;
}
```

We start by decrement the len_malloc in order to not overwrite the last NULL-char. \*The order of writing if from the right to the left.\* After, we put the result of the `nbr % 10` in order to
get the last digit of a number. For exemple, `1233`: the last digit will be `3`. So we get, decimal `3` + the letter `'0'` \*(or decimal 48)\_, we get the letter `3`.  
It is the opposite of a `ft_atoi`.
After that we divide by 10 our `nbr` to leave the last digit.

And we return the string `res`, `return (res);`.

<a name="ft_lstadd_back"></a>

## ft_lstadd_back

This function aims to add at the end of a chained list a new element. We check firstly if the pointer of pointer of t_list exists with `if (lst)` and also if the pointer of t_list exists with `if (*lst)`.  
We have declared at the beginning a temporary pointer of t_list `last`. We initialize it by calling the function `ft_lstlast(*lst)`. This function returns the address of the last element of a chained list.  
So `last` will be initialize to the address of the last item of the actual chained list. And then, we can add the item pointed by `new` with `last->next = new`.  
If `*lst` is not initialized `*lst = new`.

<a name="ft_lstadd_front"></a>

## ft_lastadd_front

Nearly the same of `ft_lastadd_back` without the temporary pointer of t_list and the call of the `ft_lastadd_back` function. And instead of modifying lst we change new by setting the `next` variable
to the address of lst: `new->next = *lst;`.  
Then we get the new address of the start by doing `*lst = new;`.

<a name="ft_lstclear"></a>

## ft_lstclear

This function uses a pointer of function. So, we initialize a temporary pointer of t_list `temp`. That will allow us to go the next item after having delete the actual item.  
The main loop looks like this:

```
while (lst && *lst)
{
    temp = (*lst)->next;
    ft_lstdelone(*lst, del);
    *lst = temp;
}
```

So, we call `ft_lstdelone(*lst, del)` in order the delete the `content` variable of the t_list. And free the actual item.

<a name="ft_lstdelone"></a>

## ft_lstdelone

This function aims to delete an item of a t_list by using its address. We delete the content of it by calling the function using the pointer of function passed in parameter. And we free the item.

<a name="ft_lstiter"></a>

## ft_lstiter

This functions aims to apply a function on each element of a chained_list. The iteration is made by using the `lst = lst->next;`.  
`next` will be set as NULL when the list is finished, and so `lst` will be NULL at the end, like that, the loop will end.

<a name="ft_lstmap"></a>

## ft_lstmap

The purpose of this function is to create a linked list containing the results of the applications of the function `f` passed as a parameter to the list `lst`.  
If something goes wrong, we use del in order to delete the content and then `ft_lst_clear` in order to delete and free the memory of all following items.
We use a `void *` in order to store the address of the result of the function `f`. The idea is to delete the element in a easier way if something goes wrong.

So, `while (lst)`, we intiliaze the pointer `new` by calling `ft_lstnew` that will malloc the item, initialize the content variable as the value pass in parameter : the call of the function `f`.  
Then, we call `ft_lastadd_back` in order to put the freshly new item `new`. And finally, we iterate.  
On the road, if something goes wrong, we delete all items in `res` et free them. We set `lst` to NULL and return NULL.

<a name="memory_functions"></a>

## Memory functions

The casts to `char *` or `unsigned char *` are very important in order to have a byte by byte iteration. We want to have the correct comparison in our while and if statements.

<a name="ft_memmove"></a>

## ft_memmove

This function is useful in scenarios where the source and destination memory areas might overlap.

- if `dst` it the same as `src`, the functions returns immediately, as no action is needed.
- if `dst` is located after `src` in memory, the functions copies the bytes from the end to the beginning to avoid overwriting the source.
- if `dst` if located before `src`, the function copies the bytes from the beginning to the end.

### overlap explanation

```
int main(void)
{
    char str[] = "Bonjour tout le monde !";

    ft_memmove(str, str + 8, 4);
    printf("%s\n", str);
}
```

In this code, as the destination `str` overlaps with the source `str + 8`, a simple left-to-right copy (like `memcpy`) could overwrite the data before it is copied completely.  
`ft_memmove` detects this overlap and copies from the right to the left (starting from the end) when the destination is find after the source in the memory. This helps preserve the date when copying.
