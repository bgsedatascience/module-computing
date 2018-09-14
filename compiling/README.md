# Compiling Code

As an example, we will compile the still-in-the-works "3.8" branch of Python, directly from source code. 

The first step is to get the source code on our computer:

```{sh}
git clone https://github.com/python/cpython
cd cpython
```

Now, following the instructions given on the Github page, we know they provide us with a script called "configure," which we should call first: 

```{sh}
./configure
```

This should report to us that there is an error! We do not have a C compiler installed. 

We can install this, and more, in an APT package called "build-essential":

```{sh}
sudo apt install build-essential
```

This package will install several programs that we need, in addition to a C compiler, it will also install a program called "make" that we will use to simple call the compiler with all the necessary arguments (without us needing to know them!):

``` shell
make
```

This will automatically run the "default" build script provided by the developers in the Makefile (which is just instructions for make, such that the program knows which source code files to compile)

Congrats, you should now have a program that you can use: 

``` shell
./python
```

Unfortunately, we need to be in the same folder as this program in order to use it. This is why we will want to put it onto our PATH somehow. One way to do this is with a symbolic link:

``` shell
ln -s $PWD/python /usr/local/bin/python3.8
```
