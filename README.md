# Honggfuzz+
## Getting started


## Building and running both strucutres of HonggFuzz+ i.e. SPHongg and FLHongg as the mutator of the AFL++ fuzzing process
### Presteps

You may need to install neccessary updates and tools at the begining:

```
sudo apt-get update
sudo apt install build-essential
```
The series of commands below are used to clone the Ninja repository from GitHub, build the Ninja executable, and install it on your system. 

```
git clone https://github.com/ninja-build/ninja.git && cd ninja
./configure.py --bootstrap
```

Then, you can copy the compiled Ninja executable to the /usr/bin/ directory on your system. The sudo command is used to run the copy operation with administrative privileges, as copying to bin directory typically requires elevated permissions.

```
sudo cp ninja /usr/bin/
```

Finally, you can check the version of the installed Ninja executable by running ninja --version. This will display the version information on the console.

```
ninja --version
```


### AFL++ installation

The most recent version of the AFL++ fuzzer for this experiment is utilized.

Install the required dependencies by executing the following commands [1]:

```
sudo apt-get update
sudo apt-get install -y build-essential python3-dev automake git flex bison libglib2.0-dev libpixman-1-dev python3-setuptools
sudo apt-get install -y lld-11 llvm-11 llvm-11-dev clang-11 || sudo apt-get install -y lld llvm llvm-dev clang
sudo apt-get install -y gcc-$(gcc --version|head -n1|sed 's/.* //'|sed 's/\..*//')-plugin-dev libstdc++-$(gcc --version|head -n1|sed 's/.* //'|sed 's/\..*//')-dev
```

Checkout and build AFL++ [1]: 

```
cd $HOME
git clone https://github.com/AFLplusplus/AFLplusplus && cd AFLplusplus
export LLVM_CONFIG="llvm-config-11"
make distrib
sudo make install
```


Assuming the previous steps were successful, you should now be able to run afl-fuzz. Simply type [1]:

```
afl-fuzz
```
This should display something similar to the following output.

<p align="center">
  <img src="https://github.com/sbamohabbatchafjiri/Honggfuzzplus/assets/47651730/7b2d92a4-dae0-4af0-9185-78bce6ae414e" alt="Image 1" width="700">
</p>
For  more details about AFL++ installation and docker file installation please visit: [AFL++](https://github.com/AFLplusplus/AFLplusplus)

### GNUplot
Installing GNUplot to generate some pretty graphs for any active fuzzing task using afl-plot. 

**Create the output directory:**

```
mkdir /home/kali/graph_output_dir
```

 Install gnuplot:

```
sudo apt-get install gnuplot
```

Then

```
sudo apt install libgtk-3-0 libgtk-3-dev pkg-config
cd /home/kali/.local/share/Trash/files/AFLplusplus/utils/plot_ui
make
cd ../..
sudo make
```

 
### Download and build your target

As you can use differet target for fuzzing, we represent a general representation of what you need to do. To access more targets and more details about sample targets, please visit the [Fuzzing101](https://github.com/antonio-morales/Fuzzing101/tree/main) website created by Antonio Morales [1].

```
cd $HOME
mkdir fuzzing_target && cd fuzzing_target/
```

**Download the target**:

```
wget https://dl.from_official_web_page.com/target.tar.gz
tar -xvzf target.tar.gz
```

Build The target:

```
cd target
CC=afl-clang-fast or afl-clang-lto # if it is required at this stage
CXX=afl-clang-fast++ or afl-clang-lto++ # if it is required at this stage
./configure --prefix="$HOME/fuzzing_target/install/"
make
make install
```

To see more details about building each target, and how to create the directory of target examples, please visit the [Fuzzing101](https://github.com/antonio-morales/Fuzzing101/tree/main) website.

**Test the build**

To ensure everything is working properly, you can test each library by running the respective command lines provided below. Although they may have slight variations, they follow a similar form. For eample libexif is as follows:

```
$HOME/fuzzing_libexif/install/bin/exif
```

By executing these command lines, you can test and verify that each library is functioning correctly.
### Enabling Custom Mutators:

**Creating .so file**

The ".so" stands for "shared object." Shared libraries contain reusable code and data that can be dynamically linked by programs at runtime. They provide a way to share code among multiple executable files, reducing duplication and improving efficiency. To create .so file you may run commandlines as follows:

```
cd AFLplusplus/custom_mutators/honggfuzz/
ls
```
Then, if there is .so file inside the honggfuzz directory, remove it and make it with current files.

```
make
```

This should display something similar to the following output.


<p align="center">
  <img src="https://github.com/sbamohabbatchafjiri/Honggfuzzplus/assets/47651730/6e17e08a-9888-4f77-a1b6-bc29ce76a844" alt="Image 2" width="700">
</p>

You can do similar process for libfuzzer as depicted in the following output:

<p align="center">
  <img src="https://github.com/sbamohabbatchafjiri/Honggfuzzplus/assets/47651730/9c962f03-6e0a-4931-9002-a0b07bf9a52f" alt="Image 3" width="700">
</p>

To create a shared object file (.so) from the provided source code and files, compile the code using a suitable compiler, such as GCC, and link it with the required libraries, following the instructions specified in the provided Makefile. This process will produce a dynamic library (.so) that encapsulates the functionality defined in the source code, allowing it to be dynamically linked and used by other programs.


**Employing and unemploying the AFL++ mutation and custom mutator**

If you want to exclusively use a custom mutator, you can specify the path to the respective shared object file as follows.

For AFL++ (American Fuzzy Lop Plus Plus) using the example code:
```
export AFL_CUSTOM_MUTATOR_ONLY="/home/kali/AFLplusplus/example.so"
```

For Honggfuzz:
```
export AFL_CUSTOM_MUTATOR_ONLY="/home/kali/AFLplusplus/custom_mutators/honggfuzz/honggfuzz-mutator.so"
```

For libfuzzer:
```
export AFL_CUSTOM_MUTATOR_ONLY="/home/kali/AFLplusplus/custom_mutators/libfuzzer/libfuzzer-mutator.so"
```

If you want to combine AFL mutation with custom mutation, you may use the following configuration.

For AFL++ (American Fuzzy Lop Plus Plus) using a example custom mutator library called example.c:
```
gcc -shared -o example.so example.c
export AFL_CUSTOM_MUTATOR_LIBRARY="/home/kali/AFLplusplus/example.so"
```

For AFL++ using the HonggFuzz custom mutator library:
```
export AFL_CUSTOM_MUTATOR_LIBRARY="/home/kali/AFLplusplus/custom_mutators/honggfuzz/honggfuzz-mutator.so"
```

For AFL++ the LibFuzzer custom mutator library:
```
export AFL_CUSTOM_MUTATOR_LIBRARY="/home/kali/AFLplusplus/custom_mutators/libfuzzer/libfuzzer-mutator.so"
```

Please note that the paths specified above are examples and should be adjusted according to the actual location of the shared object files on your system.

The provided commands assume that you have already set up the necessary directories and installed the required tools and dependencies for the respective fuzzing targets. After setting the appropriate environment variables, you can execute the fuzzing process using the respective tools.

```
afl-fuzz -i input_dir -o output_dir -- ./target_program @@
```
In our example, the command line can be something like below when -s 123 sets a fixed seed.
```
afl-fuzz -i $HOME/fuzzing_target/target_examples/ -o $HOME/fuzzing_xpdf/out/ -s 123 -- $HOME/fuzzing_target/install/bin/target @@ $HOME/fuzzing_target/output
```
Here are different senarios:


When you are using AFL++ without custom mutator and without -D, it will be like this:

<p align="center">
  <img src="https://github.com/sbamohabbatchafjiri/Honggfuzzplus/assets/47651730/a9d2bb80-8be9-43d7-be12-354a6476d130" alt="Image 4" width="700">
</p>

When you are using ALF++ without custom mutator and with -D, it will be like this:

<p align="center">
  <img src="https://github.com/sbamohabbatchafjiri/Honggfuzzplus/assets/47651730/cbe9adc3-0cf0-450f-917b-a572c466c774" alt="Image 5" width="700">
</p>



When you are using ALF++ with custom mutator and without -D, it will be like this:

<p align="center">
  <img src="https://github.com/sbamohabbatchafjiri/Honggfuzzplus/assets/47651730/c4987d00-a196-417f-8024-a9051693d152" alt="Image 6" width="700">
</p>


**Employing and unemploying a patched custom mutator**

To employ a patched custom mutator in AFL++, follow these steps:

1. Delete the existing "mangle.c" and ".so" files for the baseline HonggFuzzin already exist in the /home/kali/AFLplusplus/custom_mutators/honggfuzz/ directory.

2. Download either the "mangle(SPHongg).c" or "mangle(FLHongg).c" file and move the downloaded file ("mangle(SPHongg).c" or "mangle(FLHongg).c") to the same directory.

<p align="center">
  <img src="https://github.com/sbamohabbatchafjiri/Honggfuzzplus/assets/47651730/9b365b40-599e-44a0-ba0d-a1ce16c81a2f" alt="Image 7" width="700">
</p>

3. Rename the new file to mangle.c

<p align="center">
  <img src="https://github.com/sbamohabbatchafjiri/Honggfuzzplus/assets/47651730/910dae73-a524-401d-b84b-63e453aacfea" alt="Image 7" width="700">
</p>

4. Compile the new custom mutator file to create a new shared object (.so) file by make as explained in the previous section. Use an appropriate compiler, such as GCC, and ensure the necessary dependencies are met. The exact compilation command will depend on the specific code and any required libraries.

5. After successfully creating the new ".so" file, set the environment variable "AFL_CUSTOM_MUTATOR_ONLY" to the path of the custom mutator shared object. Use the following command:

```
export AFL_CUSTOM_MUTATOR_ONLY="/home/kali/AFLplusplus/custom_mutators/honggfuzz/honggfuzz-mutator.so"
```
6. Navigate to the directory of your target program.

By previous steps, you replaced the existing "mangle.c" and ".so" files with the patched custom mutator file and created a new shared object. Then, you employed the custom mutator by setting the "AFL_CUSTOM_MUTATOR_ONLY" environment variable before running AFL with your target program.

7. Now, run AFL using the appropriate command-line options and the path to the target program. For example:

```
afl-fuzz -i input_dir -o output_dir -- ./target_program @@
```

Replace "input_dir" with the directory containing the input files, "output_dir" with the desired output directory and and "target_program" with the name or path to your target program. 

Also you can utulize parallel fuzzing by enabling -M and -S. Just run the first one ("master", -M) like this:

$ ./afl-fuzz -i testcase_dir -o sync_dir -M fuzzer01 [...other stuff...]

and then, start up secondary (-S) instances like this:

$ ./afl-fuzz -i testcase_dir -o sync_dir -S fuzzer02 [...other stuff...]
$ ./afl-fuzz -i testcase_dir -o sync_dir -S fuzzer03 [...other stuff...]

For more details about parallel fuzzing please see [Paralel Fuzzing](https://github.com/stribika/afl-fuzz/blob/master/docs/parallel_fuzzing.txt) [3].

### Analyzing results:

1- Capturing screenshots from the AFL++ screen and manually inserting data into an Excel file to plot results every 24 hours. Here are two captured screenshots:

I. Verification stage: 3 parallel run on VMware® Workstation 16 player with 2 GB RAM and two different Operating Systems (5.19.0-kali2-amd64 (2 runs), 6.0.0-kali3-amd64 (1 run). 
Host OS: 64-bit Windows 10 Education (16 GB RAM, Intel 11th generation i5-1145G7 CPU)

<p align="center">
  <img src="https://github.com/sbamohabbatchafjiri/Honggfuzzplus/assets/47651730/a98987a6-147b-43f7-afe6-4ebe34c4f2d2)" alt="Image 5" width="700">
</p>

II. Validation stage:  10 paralel runs on 10 identical VM snapshots with 2 GB RAM (version 16.3.4)
Host OS: Windows 64GB RAM, 11 Pro with 10 CPU cores (i9-9820X CPU) of 3.30GHz  
VM OS: kali 6.0.0-kali3-amd64

<p align="center">
  <img src="https://github.com/sbamohabbatchafjiri/Honggfuzzplus/assets/47651730/ba7e4ac1-f246-4835-91ac-3110438ed78e)" alt="Image 5" width="700">
</p>


2- By fetching data from afl-whatsup and calculating the time, you may evaluate if the time interval between run-time and last seen hang is longer than 24 hours. To do this, you may navigate to the directory where your target's fuzzing campaign is located. This directory should contain the AFL output directory (out/). Run the afl-whatsup command followed by the path to the AFL output directory as follows:
```
afl-whatsup out/
```

This command will execute afl-whatsup on the out/ directory, providing the status summary of the AFL instances in that directory.

last_hang (time interval between run-time and last seen hang) is shown as below:

<p align="center">
  <img src="https://github.com/sbamohabbatchafjiri/Honggfuzzplus/assets/47651730/3053b1d9-5f1f-4418-bef3-d53b53a2dca0" alt="Image 5" width="700">
</p>

```
import time
# dictionary to store the last seen crash/hang time for each machine
last_seen = {
    "machine1": time.time(),
    "machine2": time.time(),
    "machine3": time.time(),
    "machine4": time.time(),
    "machine5": time.time(),
    "machine6": time.time(),
    "machine7": time.time(),
    "machine8": time.time(),
    "machine9": time.time(),
    "machine10": time.time(),
}

# list to store the timestamps of when each alarm was triggered
alarm_timestamps = []

while True:
    time.sleep(3599*3599)  # wait for 23 hours and 59 minutes and 59 seconds
    
    # calculate the time gap between the last seen crash/hang and current time for each machine
    time_gaps = [time.time() - last_seen[machine] for machine in last_seen]
    
    # count the number of machines that have exceeded the 24-hour threshold
    machines_exceeded_threshold = sum(1 for gap in time_gaps if gap >= 24*60*60)
    
    # check if 5 or more machines have exceeded the threshold and trigger the first alarm
    if machines_exceeded_threshold >= 5 and len(alarm_timestamps) == 0:
        alarm_timestamps.append(time.time())
        print("First alarm triggered at", time.ctime())
    
    # check if 8 or more machines have exceeded the threshold and trigger the second alarm
    elif machines_exceeded_threshold >= 8 and len(alarm_timestamps) == 1:
        alarm_timestamps.append(time.time())
        print("Second alarm triggered at", time.ctime())
        break  # exit the loop
    
print("Alarm timestamps:", [time.ctime(ts) for ts in alarm_timestamps])


}
```
The term "Last seen" refers to the time at which the last significant event (either a crash or a hang) occurred during the fuzzing process. The "Last seen" value can be based on either the last_crash or last_hang occurance time, depending on the chosen strategy for determining testing time and analyzing graphs. In afl-whatsup the time gap between run time and last seen crash or hang is saved in last_crash or last_hang. No need to calulate it again. It is as follows: 

```
import time
# dictionary to store the last seen crash/hang time for each machine
last_hang = {
    "machine1": time.time(),
    "machine2": time.time(),
    "machine3": time.time(),
    "machine4": time.time(),
    "machine5": time.time(),
    "machine6": time.time(),
    "machine7": time.time(),
    "machine8": time.time(),
    "machine9": time.time(),
    "machine10": time.time(),
}

# list to store the timestamps of when each alarm was triggered
alarm_timestamps = []

while True:
    time.sleep(3599*3599)  # wait for 23 hours and 59 minutes and 59 seconds
    
    # calculate the time gap between the last seen crash/hang and current time for each machine
    time_gaps = [last_hang[machine] for machine in last_hang]
    
    # count the number of machines that have exceeded the 24-hour threshold
    machines_exceeded_threshold = sum(1 for gap in time_gaps if gap >= 24*60*60)
    
    # check if 5 or more machines have exceeded the threshold and trigger the first alarm
    if machines_exceeded_threshold >= 5 and len(alarm_timestamps) == 0:
        alarm_timestamps.append(time.time())
        print("First alarm triggered at", time.ctime())
    
    # check if 8 or more machines have exceeded the threshold and trigger the second alarm
    elif machines_exceeded_threshold >= 8 and len(alarm_timestamps) == 1:
        alarm_timestamps.append(time.time())
        print("Second alarm triggered at", time.ctime())
        break  # exit the loop
    
print("Alarm timestamps:", [time.ctime(ts) for ts in alarm_timestamps])


}
```

3- analysing afl-gnu graphs by below command lines:
The commands generate graphical plots using `afl-plot` for different fuzzing campaigns. Here's a short explanation of how to run each command:

I. Plot for XPDF:
   ```
   /usr/local/bin/afl-plot --graphical /home/kali/fuzzing_xpdf/out/default/ /home/kali/graph_output_dir
   ```
   This command generates a graphical plot for the fuzzing campaign conducted on XPDF. It assumes that the fuzzing output directory is located at `/home/kali/fuzzing_xpdf/out/default/`. The generated plot will be saved in the directory specified by `/home/kali/graph_output_dir`.

II. Plot for Libtiff:
   ```
   /usr/local/bin/afl-plot --graphical /home/kali/fuzzing_tiff/out/default /home/kali/graph_output_dir
   ```
   This command generates a graphical plot for the fuzzing campaign conducted on Libtiff. It assumes that the fuzzing output directory is located at `/home/kali/fuzzing_tiff/out/default`. The resulting plot will be saved in the directory specified by `/home/kali/graph_output_dir`.

III. Plot for tcpdump:
   ```
   /usr/local/bin/afl-plot --graphical /home/kali/fuzzing_tcpdump/out/default /home/kali/graph_output_dir
   ```
   This command generates a graphical plot for the fuzzing campaign conducted on tcpdump. It assumes that the fuzzing output directory is located at `/home/kali/fuzzing_tcpdump/out/default`. The generated plot will be saved in the directory specified by `/home/kali/graph_output_dir`.

Make sure to replace `/home/kali/graph_output_dir` with the actual directory path where you want to save the generated plots.

Here are examples of verification and validation stages.

**Verification stage:**

<p align="center">
  <img src="https://github.com/sbamohabbatchafjiri/Honggfuzzplus/assets/47651730/b1edc626-33c3-4129-b504-9a73346e190e)" alt="Image 5" width="700">
</p>


**Validation stage:**

<p align="center">
  <img src="https://github.com/sbamohabbatchafjiri/Honggfuzzplus/assets/47651730/1651816a-eba1-4a83-8d8b-3a852046d09d)" alt="Image 5" width="700">
</p>

Experiment results are available in ...

## Refrences

[1] Antonio Morales. Fuzzing101. [Online]. Available: [Fuzzing101 GitHub Repository](https://github.com/antonio-morales/Fuzzing101/tree/main). Accessed: [09/06/2023].

[2] Andrea Fioraldi, Dominik Maier, Heiko Eißfeldt, and Marc Heuse. “AFL++: Combining incremental steps of fuzzing research”. In 14th USENIX Workshop on Offensive Technologies (WOOT 20). USENIX Association, AFL++ [Online]. Available: [AFL++](https://github.com/AFLplusplus/AFLplusplus). Aug. 2020. Accessed: [09/06/2023].

[3] [Paralel Fuzzing](https://github.com/stribika/afl-fuzz/blob/master/docs/parallel_fuzzing.txt) - Stribika, "Tips for parallel fuzzing" repository, Accessed [09/06/2023].


## Contact
