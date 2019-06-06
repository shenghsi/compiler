### Notes
```
syntatic:
scaned token matches wiht grammar in parser

semantic:
After syntatic match, check:
1. are variables declared in current scope?
2. are variables compatible for the current operator?
3. is result compatible with left-hand side?
```

### Download and install the C++ runtime for ANTLR
```bash
Go to https://github.com/antlr/antlr4 and click the green Clone or download button. Then click the
Download ZIP button. Save the downloaded zip anrlr4-master.zip in your home directory and unzip it.
You will get a directory antlr4-master. Change to directory antlr4-master/runtime/Cpp. Go to
https://github.com/antlr/antlr4/tree/master/runtime/Cpp and see the instructions under “Compiling on
Linux”. Type the following in the Ubuntu terminal window:
mkdir build && mkdir run && cd build
cmake .. -DANTLR_JAR_LOCATION=/Users/shxi/omscs/CS8803compiler/Summer_Compilers_Project_1/antlr/antlr-4.7.2-complete.jar -DWITH_DEMO=True
make
DESTDIR=../run make install

The last command above will take a while to approach 100% completion.
Enter subdirectory antlr4-master/runtime/Cpp/run/usr/local/include. Type the following command to
copy the directory containing the ANTLR header files to the standard system location:
sudo cp -r antlr4-runtime /usr/local/include

Enter subdirectory antlr4-master/runtime/Cpp/run/usr/local/lib. Type the following commands to
copy the ANTLR libraries to the standard system location and make them available to programs:
sudo cp * /usr/local/lib
sudo ldconfig

Now you can change to your home directory and delete all of directory antlr4-master:
rm -r antlr4-mast

```
