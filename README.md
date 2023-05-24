# Java Utils for Termux
A collection of useful scripts to work with Java 17 on Termux.

Of course, you can also run it in any other linux distro apart from
Termux or Android. Not compatible with Windows.

# Installation

First of all, you have to install Java to work with it.
OpenJDK-17 is available in Termux repositories, install it with:
```
pkg install openjdk-17
```

After that, clone the repo, add the repository path
to your `$PATH` variable, for example using rc or profile files.

If you are lazy, you can copy the following code,
depending on which shell you use. First `cd` into the repo,
run
```
rc="<your_shells_rc_file_path>"
```
then run
```
echo "export PATH=\$PATH:$PWD" >> $rc
```

# Usage

All your libraries have to be `*.jar` files inside `libs/` directory.
When writing source code, use the directory `src/`, the compiled
output will be generated in the `out/` directory or in the root
of your project if it is a jar file.

All the utils are called `j<util_name>` for example `jinit` for `J init`

## J init

Creates fresh `src/` and `libs/` folders. This will
delete anything from the directories, if they were
already present.

## J create

This util should be used to create new files. This will
take care of the package and the class name. `main` argument
can be passed to add a main method to the class.
The file argument should be given like a java path,
for example `com.gxlg.hello.Main`.

## J import

Tries to compile the project, and throws which imports
should have been added to the source code.

It searches the `java_classlist` file for most common java
classes and the `libs/` directory for libraries imports.

Prints `[!] <class> not found` if none have been found. If
it happens, please check your `libs/` folder to contain
the needed library.

## J make

Compiles the source to the output folder `out/`.
Then it automatically searches for any
`public static void main(String[])` methods in the code.
Following can happen:
* You have only one `main` method: very good, the util will
run and create a `run` script to run your code
* You have multiple `main` methods: probably you are testing something
or just need different entries for different scenarios, not
a problem for the util: The util will run, create a `run` script
with all possible entries listed but commented out, then the util
will open `nano` for you to chose which line to uncomment, so the
correct entry point is selected
* You have no `main` methods: what a pity! You forgot something,
or maybe you are trying to create a library, the util will not create
any `run` script this time

Any arguments which can be passed to `javac` can be as well
passed to this util.

## J jar

Compiles the source to the root folder to a file called `out.jar`.
Same mechanics apply here as in `J make`, except the run file
is not being created and instead a temporary manifest contains
the entry point.

Any arguments which can be passed to `javac` can be as well
passed to this util.

## J lib

Compiles the source to the root folder to a file called `lib.jar`.
This is used to create a library, which can be used from
inside the `libs/` directory, no entry points are needed for this.

Any arguments which can be passed to `javac` can be as well
passed to this util.
