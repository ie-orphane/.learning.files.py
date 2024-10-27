Variables are a fine way to store data while your program is running, but if you want your data to persist even after your program has finished, you need to save it to a file. You can think of a file’s contents as a single string value, potentially gigabytes in size. In this chapter, you will learn how to use Python to create, read, and save files on the hard drive.

### Files and Paths
	
A file has two key properties: a filename (usually written as one word) and a path.

The path specifies the location of a file on the computer.

For example, there is a file on my Windows 7 laptop with the filename **projects.docx** in the path **C:\\Users\\La7ya\\Documents**.

The part of the filename after the last period is called the file’s **extension** and tells you a file’s type. 

**project.docx** is a Word document, and **Users**, **La7ya**, and **Documents** all refer to folders (also called **directories**).

Folders can contain files and other folders.

For example, **project.docx** is in the Documents folder, which is inside the **La7ya** folder, which is inside the Users folder. Figure 8 shows this folder organization.

The C:\ part of the path is the root folder, which contains all other folders.

On Windows, the root folder is named C:\ and is also called the C: drive. On OS X and Linux, the root folder is /.


```
C:\
  |_Users
    |_La7ya
      |_Documents
        |_project.docx
```
Figure 8-1: A file in a hierarchy of folders on **Windows**

```
/
  |_Users
    |_La7ya
      |_Documents
        |_project.docx
```
Figure 8-2: A file in a hierarchy of folders on **OS X** and **Linux**


On Windows, paths are written using backslashes (**\\**) as the separator between folder names.

OS X and Linux, however, use the forward slash (**/**) as their path separator. 

If you want your programs to work on all operating systems, you will have to write your Python scripts to handle both cases using the **os.path.join()** function.

``` python
>>> import os 
>>> os.path.join('usr', 'bin', 'spam') 
'usr\\bin\\spam' # on Windows
'usr/bin/spam' # on OS X and Linux
```

### The Current Working Directory

Every program that runs on your computer has a current working directory, or cwd.

Any filenames or paths that do not begin with the root folder are assumed to be under the current working directory.

You can get the current working directory as a string value with the **os.getcwd**() function and change it with **os.chdir**().

```python
>>> import os 
>>> os.getcwd()
'C:\\Users\\La7ya'
>>> os.chdir('C:\\Windows\\System32')
>>> os.getcwd()
'C:\\Windows\\System32'
```

 Here, the current working directory is set to **C:\\Users\\La7ya**, so the filename **project.docx** refers to **C:\\Python34\\project.docx**.
 
 When we change the current working directory to **C:\\Windows**, project.docx is interpreted as C:\\ Windows\\project.docx.
 
 Python will display an error if you try to change to a directory that does not exist. 
```python
>>> os.chdir('C:\\ThisFolderDoesNotExist')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
FileNotFoundError: [WinError 2] The system cannot find the file specified: 'C:\\ThisFolderDoesNotExist'
```
 
 NOTE:  While folder is the more modern name for directory, note that current working directory (or just working directory) is the standard term, not current working folder.


### Absolute vs. Relative Paths

There are two ways to specify a file path.
	• An absolute path, which always begins with the root folder
	• A relative path, which is relative to the program’s current working directory 
	
There are also the dot (.) and dot-dot (..) folders. These are not real folders but special names that can be used in a path.

A single period (“dot”) for a folder name is shorthand for “this directory”, and two periods (“dot-dot”) means “the parent folder.”

Figure 9 is an example of some folders and files. When the current working directory is set to **C:\\foo**, the relative paths for the other folders and files are set as they are in the figure.

The **./** at the start of a relative path is optional. For example, ./spam.txt and spam.txt refer to the same file.

```console
Folders hireochy    Relative paths     Absolute paths

  /                  ../                 /
  |_foo (cwd)        ./                  /foo
    |_spam           ./spam              /foo/spam
      |_main.py      ./spam/main.py      /foo/spam/main.py
    |_README.md      ./README.md         /foo/README.md
  |_bar              ../bar              /bar
    |_spam.txt       ../bar/spam.txt     /bar/spam.txt
```
Figure 9: The relative paths for folders and files in the working directory **foo** 

### Finding File Sizes and Folder Contents

Once you have ways of handling file paths, you can then start gathering information about specific files and folders.

The **os** library provides functions for finding the size of a file in bytes and the files and folders inside a given folder.

- Calling **os.path.getsize(path)** will return the size in bytes of the file in the path argument.
- Calling **os.listdir(path)** will return a list of filename strings for each file in the path argument. (Note that this function is in the os module, not os.path.)

```python
>>> os.path.getsize('C:\\Windows\\System32\\calc.exe')
27648
>>> os.listdir('C:\\Windows\\System32')
['0409', '12520437.cpx', '12520850.cpx', '5U877.ax', 'aaclient.dll',
...
'xwtpdui.dll', 'xwtpw32.dll', 'zh-CN', 'zh-HK', 'zh-TW', 'zipfldr.dll']
```

### Checking Path Validity

Many Python functions will crash with an error if you supply them with a path that does not exist. The os.path module provides functions to check whether a given path exists and whether it is a file or folder.

- Calling **os.path.exists(path)** will return True if the file or folder referred to in the argument exists and will return False if it does not exist.
- Calling **os.path.isfile(path)** will return True if the path argument exists and is a file and will return False otherwise.
- Calling **os.path.isdir(path)** will return True if the path argument exists and is a folder and will return False otherwise.
	
```python
>>> os.path.exists('C:\\Windows')
True
>>> os.path.exists('C:\\some_made_up_folder')
False
>>> os.path.isdir('C:\\Windows\\System32')
True
>>> os.path.isfile('C:\\Windows\\System32')
False
>>> os.path.isdir('C:\\Windows\\System32\\calc.exe')
False
>>> os.path.isfile('C:\\Windows\\System32\\calc.exe')
True
```

### The File Reading/Writing Process

There are three steps to reading or writing files in Python.
1. Call the open() function to return a File object. 
2. Call the read() or write() method on the File object. 
3. Close the file by calling the close() method on the File object.

#### Opening and Closing of Files

```python
# manual opening and closing of file
hello_file = open('C:\\Users\\La7ya\\Desktop\\hello.txt')
# If you’re using OS X, or Linux:
hello_file = open('/Users/La7ya/Desktop/hello.txt')

hello_file.close()

# automate closing of file
with open('C:\\Users\\La7ya\\Desktop\\hello.txt') as hello_file:
	# do someting later
```

#### Reading the Contents of Files

```python
with open('C:\\Users\\La7ya\\Desktop\\hello.txt') as hello_file:
	file_content = hello_file.read()
	print(file_content)
```
```console
Salam Al3alam!
```

```python
with open('C:\\Users\\La7ya\\Desktop\\large.txt') as large_file:
	file_lines = large_file.readlines()
	print(file_lines)
```
```console
['When, in disgrace with fortune and men's eyes,\n', ' I all alone beweep my outcast state,\n', And trouble deaf heaven with my bootless cries,\n', And look upon myself and curse my fate,']
```
### Writing to Files

Python allows you to write content to a file in a way similar to how the print() function “writes” strings to the screen.

You can’t write to a file you’ve opened in read mode, though.

Instead, you need to open it in “write plaintext” mode or “append plaintext” mode, or write mode and append mode for short.

Write mode will overwrite the existing file and start from scratch, just like when you overwrite a variable’s value with a new value. 

Pass 'w' as the second argument to open() to open the file in write mode.

Append mode, on the other hand, will append text to the end of the existing file.

You can think of this as appending to a list in a variable, rather than overwriting the variable altogether.

Pass 'a' as the second argument to open() to open the file in append mode.

If the filename passed to open() does not exist, both write and append mode will create a new, blank file.

After reading or writing a file, call the close() method before opening the file again.

```python
>>> baconFile = open('bacon.txt', 'w')
>>> baconFile.write('Hello world!\n')
13
>>> baconFile.close()
>>> baconFile = open('bacon.txt', 'a')
>>> baconFile.write('Bacon is not a vegetable.')
25
>>> baconFile.close() 
>>> baconFile = open('bacon.txt')
>>> content = baconFile.read()
>>> baconFile.close()
>>> print(content)
Hello world! Bacon is not a vegetable.
```

First, we open bacon.txt in write mode. Since there isn’t a bacon.txt yet, Python creates one. Calling write() on the opened file and passing write() the string argument 'Hello world! /n' writes the string to the file and returns the number of characters written, including the newline. Then we close the file.

To add text to the existing contents of the file instead of replacing the string we just wrote, we open the file in append mode. We write 'Bacon is not a vegetable.' to the file and close it. Finally, to print the file contents to the screen, we open the file in its default read mode, call read(), store the resulting File object in content, close the file, and print content.

Note that the write() method does not automatically add a newline character to the end of the string like the print() function does. You will have to add this character yourself.

### JSon and APis

**JavaScript Object Notation** is a popular way to format data as a single **human-readable** string.

JSON is the native way that JavaScript programs write their data structures and usually resembles what Python’s pprint() function would produce.

You don’t need to know JavaScript in order to work with JSON-formatted data.

Here’s an example of data formatted as JSON:
```json
{
	"name": "Zophie",
	"isCat": true,
	"miceCaught": 0,
	"napsTaken": 37.5,
	"felineIQ": null
}
```

JSON is useful to know, because many websites offer JSON content as a way for programs to interact with the website.

This is known as providing an **application programming interface** (API).

Accessing an API is the same as accessing any other web page via a **URL**.

The difference is that the data returned by an API is formatted (with JSON, for example) for machines; APIs aren’t easy for people to read.

Many websites make their data available in JSON format.

Facebook, Twitter, Yahoo, Google, Tumblr, Wikipedia, Flickr, Data.gov, Reddit, IMDb, Rotten Tomatoes, LinkedIn, and many other popular sites offer APIs for programs to use.

Some of these sites require registration, which is almost always free. You’ll have to find documentation for what URLs your program needs to request in order to get the data you want, as well as the general format of the JSON data structures that are returned.

This documentation should be provided by whatever site is offering the API; if they have a “Developers” page, look for the documentation there.


Using APIs, you could write programs that do the following:

- Scrape raw data from websites. (Accessing APIs is often more convenient than downloading web pages and parsing HTML with Beautiful Soup.)

- Automatically download new posts from one of your social network accounts and post them to another account. For example, you could take your Tumblr posts and post them to Facebook.

- Create a “movie encyclopedia” for your personal movie collection by pulling data from IMDb, Rotten Tomatoes, and Wikipedia and putting it into a single text file on your computer.


### the json module

Python’s json module handles all the details of translating between a string with JSON data and Python values for the **json.loads()** and **json.dumps()** functions.

JSON can’t store every kind of Python value.

It can contain values of only the following data types: **strings**, **integers**, **floats**, **Booleans**, **lists**, **dictionaries**, and **NoneType**.

JSON cannot represent Python-specific objects, such as File objects, CSV Reader or Writer objects, Regex objects, or Selenium WebElement objects.

#### Reading JSON with loads()

To translate a string containing JSON data into a Python value, pass it to the json.loads() function. (The name means “load string,” not “loads.”)

Enter the following into the interactive shell:

```python
>>> stringOfJsonData = '{"name": "Zophie", "isCat": true, "miceCaught": 0, "felineIQ": null}'
>>> import json
>>> jsonDataAsPythonValue = json.loads(stringOfJsonData)
>>> jsonDataAsPythonValue
{'isCat': True, 'miceCaught': 0, 'name': 'Zophie', 'felineIQ': None}
```

After you import the json module, you can call loads() and pass it a string of JSON data. Note that JSON strings always use double quotes.

It will return that data as a Python dictionary.

Python dictionaries are not ordered, so the key-value pairs may appear in a different order when you print jsonDataAsPythonValue.

#### Writing JSON with dumps()

The json.dumps() function (which means “dump string,” not “dumps”) will translate a Python value into a string of JSON-formatted data.

```python
>>> pythonValue = {'isCat': True, 'miceCaught': 0, 'name': 'Zophie', 'felineIQ': None}
>>> import json
>>> stringOfJsonData = json.dumps(pythonValue)
>>> stringOfJsonData
'{"isCat": true, "felineIQ": null, "miceCaught": 0, "name": "Zophie" }'
```

The value can only be one of the following basic Python data types: dictionary, list, integer, float, string, Boolean, or None.

#### Reading JSON file with load()

The json.load() function reads JSON data from a file and converts it into a Python value (usually a dictionary).

Here’s how to use it:

1. Create a JSON file: First, create a file named **data.json** with the following content:

```json
{
    "name": "Zophie",
    "isCat": true,
    "miceCaught": 0,
    "felineIQ": null
}
```

2. Use the json.load() function:

```python
import json

# Open the JSON file for reading
with open('data.json', 'r') as jsonFile:
	jsonDataAsPythonValue = json.load(jsonFile)

# Print the resulting Python value
print(jsonDataAsPythonValue)
```

When you run this code, jsonDataAsPythonValue will be a Python dictionary:

```python
{'isCat': True, 'miceCaught': 0, 'name': 'Zophie', 'felineIQ': None}
```

### Writing JSON with dump()

The json.dump() function writes a Python value to a file in JSON format.

Here’s how to use it:

1. Prepare a Python dictionary:

```python
pythonValue = {
    "isCat": True,
    "miceCaught": 0,
    "name": "Zophie",
    "felineIQ": None
}
```

2. Use the json.dump() function:

```python
import json

# Open a file for writing
with open('output.json', 'w') as jsonFile:
	json.dump(pythonValue, jsonFile, indent=2)

# The JSON data is now written to output.json
```

After running this code, the output.json file will contain:

```json
{
  "isCat": true,
  "miceCaught": 0,
  "name": "Zophie",
  "felineIQ": null
}
```

##### Summary

- json.load(file): Reads JSON data from a file and converts it to a Python value.
- json.dump(value, file): Writes a Python value to a file in JSON format.

These functions are particularly useful for handling larger datasets stored in JSON files.
