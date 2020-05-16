# matlab_checkcode

_Because we all need pointers when it comes to make our codebase better._

A lot of us (at least in neuroscience) learn matlab by aping scripts we have borrowed from someone.
Since the people we take the code from are as likely as you to have taken
'How to write good code 101' (i.e very unlikely but don't feel bad those classes are usually not
proposed in most neuroscience departments anyway), this is the fastest way to end with
matlab scripts of 1000 lines or above that are a nightmare to debug or to read (for others or for
you in 6 months).

In case you want to avoid this, this function can help you files that are getting too complex or
have too few comments.

## McCabe complexity

The McCabe complexity of a function is presents how many paths one can take while navigating through
the conditional statements of your function (`if`, `switch`, ...). If it gets above 10 you enter the
danger zone. If you are above 15 you might seriously reconsider
[refactor](https://en.wikipedia.org/wiki/Code_refactoring) your code with sub-functions for example.

-   [refactoring.com](https://refactoring.com/)
-   [refactoring.guru](https://refactoring.guru/refactoring)

## check_my_code

It is function that will give you the The McCabe complexity of all the `.m` files in a folder.
It will also return the complexity of the sub-functions in each file.

This function checks the proportion of lines with comments in each file (might overestimate it).
In general you might want to try to be around 20%.

It will then list the functions that do not meet the requirements you have set for your projects.
You can then use this information to figure out which function you should refactor first.

### INPUTS

#### recursive

BOOLEAN: if set to true this will check the .m files in all the subfolders. (default: false)

#### cplx_thrs

1 X 2 ARRAY: Thresholds for the acceptable McCabe complexity before triggering a warning.
Having 2 values lets you decide a zone of complexity that is high but acceptable and another that is
too high. (default: [15 20])

#### comment_thres

1 X 2 ARRAY : thresholds for the acceptable percentage of comments in a file
    before triggering a warning.
    Having 2 values lets you decide levels that are low but acceptable and another that is
    too low. (default: [20 10])

#### print_file

BOOLEAN this will print a file with the overall error code ; mostly used for automation
    for now. (default: true)

### OUPUTS

#### error_code

#### file_function

#### cplx : 1 X 2 ARRAY

#### percentage_comment

## Installation

You can then use the [matlab package manager](https://github.com/mobeets/mpm), to simply download
this repository by typing this in the matlab prompt:

```
mpm install matlab_checkcode -u https://github.com/Remi-Gau/matlab_checkcode.git
```

This will add `check_my_code` to the matlab path, but you will have to save the path if you want to
make this permanent or run `mpm init` next time your start matlab.

## Implementation detail

It relies on the linter used natively by matlab so it could also be extended to check all the messages relating to
all the little other issues in your code that you have not told matlab to ignore.

Also because octave does not have a linter, so this will only work with matlab. 😭

## Requirements

This function will require Matlab 2017a or above to work.

Also because octave does not have a linter, so this will only work with matlab.

Any help is welcome to make this available to older Matlab version or to implement it in octave.

## Automation

If the `check_my_code` function is in the matlab path, you can automate its usage if you use Git for your project.

I have created a git hook that will execute the `check_my_code` every time you try to push to your
remote repository. If your code is not up to the standard then the push will be aborted.

To use this here is what you need to do:
1. Copy `pre-push.sample` into your `project/.git/hooks`
2. Rename it to `pre-push`
3. You might need to modify the alias matlab line below to point this script to where matlab is on your computer.
4. Make this file executable by running `chmod +x .git/hooks/pre-push`
5. Now your code quality will be checked if when you push your code
