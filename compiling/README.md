# Compiling Code

## Building from source

As an example, we will compile the super-hot-and-unreleased "3.9" branch of Python, directly from source code.

The first step is to get the source code on our computer (We'll learn all about git later, don't worry):

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

Unfortunately, we need always access this file in order to use Python. Often we like our applications to be called from anywhere. This is why we will want to put it onto our PATH somehow. One way to do this is with a symbolic link:

``` shell
ln -s $PWD/python /usr/local/bin/python3.9
```

## Downloading Binaries

In addition to compiling the source code into a binary ourself, as we did with Python, we can download pre-compiled binaries: someone else already compiled them for our system and we can just run them directly!

We will use Julia as an example of this. Go to the downloads page (what version do you think you should download for your Linux VM?):

https://julialang.org/downloads/

Get the link using wget. Try `wget help` to see all the options, it's good to know what it does, but you won't actually need any!

``` shell
wget [LINK]
```

Now use `ls` to see if you downloaded the file.

The file has two extensions: tar and gz. This is a common way to deliver compressed archives in Unix systems. The "gz" part tells us that it has been compressed. The "tar" tells us that it is an archive file.

Take a look at `tar --help` to see the options that tar has. I always forget which ones I need. In this case, we need to use "x" to extract all the folders and create directories that we need. Then we need "f" to tell it the filename!

``` shell
tar -xf julia-1.2.0-linux-x86_64.tar.gz
```

Use `ls` again to see what you've done, then use it again to look inside the folder you just created.

Binaries are traditionally stored in folders called "bin". If we look in the "bin" folder, sure enough, we have a binary called "julia".

Let's try and execute it:

``` shell
julia-1.2.0/bin/julia
```

It should run! Now you can use Julia, a beautifully but woefully underused language.

Let's create a symbolic link from this binary to our path, just like we did with Python, so that we can call "julia" from anywhere.

## Installing via a package manager

Some libraries have quite complex instructions in their "make" file. In order to work properly, they might want to create multiple folders in your system and put different files in different folders.

In that case, it's not as simple as downloading a binary. Compiling from source would work, but we want to be able to UNINSTALL everything nicely again. Also, we want to be able to upgrade the application!

The easiest way to keep track of our applications, upgrade, and uninstall them is via a package manager.

The Ubuntu/Debian package manager is called `apt`, while the Fedora/AWS Linux package manager is called `yum`.

Let's look to see if we can find "r-base", the name of the r package, in apt:

``` shell
sudo apt search r-base
```

You will see that there is an old version in the default ubuntu repositories. Repositories hold the code for applications that can be installed. We will need to add the CRAN repository to our list. Let's look at the CRAN (Madrid mirror) to see how to do that:

https://cran.rediris.es/

Click "download R for Linux" and follow the link to the Ubuntu folder. That gives you installation instructions. The following indicates a repository:

``` shell
deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran35/
```

Notice that each repository they offer is for a different major version of Ubuntu. If you're running 18 (Bionic Beaver), you should use the above repository. They recommend modifying your sources.list file directly, which is a file that lives in /etc/apt/. We will use the command `add-apt-repository` to do this for us, however:

``` shell
sudo add-apt-repository deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran35/
```

After we add a repository, we need to run `sudo apt update` in order to refresh our list of available packages. Try that now.

You should see an error related to the lack of a public key for the r-project repository. That's because we didn't add their key!

Every package is signed with a key. Many repositories have packages that are all signed with the same key. To verify the contents of the package, you need this public key to check that it was signed with the corresponding private key, kept safe by the creator of the package.

Let's add the key by following their instructions further down the page and the `apt-key` command.

Now we can try `sudo apt update` again and it should work. Now when we search for "r-base", we should find the latest (3.6) version! Now all we have to do is install it:

``` shell
sudo apt install r-base
```
