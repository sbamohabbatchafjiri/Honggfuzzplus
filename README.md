# Honggfuzz+
## Getting started


## Building and running both strucutres of HonggFuzz+ i.e. SPHongg and FLHongg as the mutator of the AFL++ fuzzing process
### Presteps

You may need to install neccessary updates and tools at the begining:

<div>
  <pre>
    <code class="language-bash">
sudo apt-get update
sudo apt install build-essential
    </code>
  </pre>
</div>
The series of commands below are used to clone the Ninja repository from GitHub, build the Ninja executable, and install it on your system. 
<div>
  <pre>
    <code class="language-bash">
git clone https://github.com/ninja-build/ninja.git && cd ninja
./configure.py --bootstrap
    </code>
  </pre>
</div>

Then, you can copy the compiled Ninja executable to the /usr/bin/ directory on your system. The sudo command is used to run the copy operation with administrative privileges, as copying to bin directory typically requires elevated permissions.
<div>
  <pre>
    <code class="language-bash">
sudo cp ninja /usr/bin/
    </code>
  </pre>
</div>

Finally, you can check the version of the installed Ninja executable by running ninja --version. This will display the version information on the console.

<div>
  <pre>
    <code class="language-bash">
ninja --version
    </code>
  </pre>
</div>


### AFL++ installation

The most recent version of the AFL++ fuzzer for this experiment is utilized.

Install the required dependencies by executing the following commands [1]:
<div>
  <pre>
    <code class="language-bash">
      <!-- Paste your code here -->
      sudo apt-get update
      sudo apt-get install -y build-essential python3-dev automake git flex bison libglib2.0-dev libpixman-1-dev python3-setuptools
      sudo apt-get install -y lld-11 llvm-11 llvm-11-dev clang-11 || sudo apt-get install -y lld llvm llvm-dev clang
      sudo apt-get install -y gcc-$(gcc --version|head -n1|sed 's/.* //'|sed 's/\..*//')-plugin-dev libstdc++-$(gcc --version|head -n1|sed 's/.* //'|sed 's/\..*//')-dev
    </code>
  </pre>
</div>

Checkout and build AFL++ [1]: 

<div>
  <pre>
    <code class="language-bash">
      <!-- Paste your code here -->
      cd $HOME
      git clone https://github.com/AFLplusplus/AFLplusplus && cd AFLplusplus
      export LLVM_CONFIG="llvm-config-11"
      make distrib
      sudo make install
    </code>
  </pre>
</div>


Assuming the previous steps were successful, you should now be able to run afl-fuzz. Simply type [1]:
<div>
  <pre>
    <code class="language-bash">
    afl-fuzz
    </code>
  </pre>
</div>
This should display something similar to the following output.

![image](https://github.com/sbamohabbatchafjiri/Honggfuzzplus/assets/47651730/7b2d92a4-dae0-4af0-9185-78bce6ae414e)

For  more details about AFL++ installation and docker file installation please visit: [AFL++](https://github.com/AFLplusplus/AFLplusplus)
### GNUplot
Installing GNUplot to generate some pretty graphs for any active fuzzing task using afl-plot. 

Create the output directory:
<div>
  <pre>
    <code class="language-bash">
mkdir /home/kali/graph_output_dir
    </code>
  </pre>
</div>
<div>
Install gnuplot:
  <pre>
    <code class="language-bash">
sudo apt-get install gnuplot
    </code>
  </pre>
</div>

Then
<div>
  <pre>
    <code class="language-bash">
sudo apt install libgtk-3-0 libgtk-3-dev pkg-config
cd /home/kali/.local/share/Trash/files/AFLplusplus/utils/plot_ui
make
cd ../..
sudo make
  </code>
  </pre>
</div>

 
### Download and build your target

As you can use differet target for fuzzing, we represent a general representation of what you need to do. To access more targets and more details about sample targets, please visit the [Fuzzing101](https://github.com/antonio-morales/Fuzzing101/tree/main) website created by Antonio Morales [1].
<div>
  <pre>
    <code class="language-bash">
      cd $HOME
      mkdir fuzzing_target && cd fuzzing_target/
 </code>
  </pre>
</div>

Download the target:
<div>
  <pre>
    <code class="language-bash">
      wget https://dl.from_official_web_page.com/target.tar.gz
      tar -xvzf target.tar.gz
    </code>
  </pre>
</div>

Build The target:
<div>
  <pre>
    <code class="language-bash">
      cd target
      CC=afl-clang-fast or afl-clang-lto # if it is required at this stage
      CXX=afl-clang-fast++ or afl-clang-lto++ # if it is required at this stage
      ./configure --prefix="$HOME/fuzzing_target/install/"
      make
      make install
    </code>
  </pre>
</div>

To see more details about building each target, please visit the [Fuzzing101](https://github.com/antonio-morales/Fuzzing101/tree/main) website.

Test the build

To ensure everything is working properly, you can test each library by running the respective command lines provided below. Although they may have slight variations, they follow a similar form. For eample libexif is as follows:


For libexif:
<div>
  <pre>
    <code class="language-bash">
$HOME/fuzzing_libexif/install/bin/exif
    </code>
  </pre>
</div>

By executing these command lines, you can test and verify that each library is functioning correctly. 


For more targets please see [Exercise 1](https://github.com/antonio-morales/Fuzzing101/tree/main/Exercise%201) created by Antonio Morales in the "Fuzzing101" repository [1].


## Refrences

[1] Antonio Morales. Fuzzing101. [Online]. Available: [Fuzzing101 GitHub Repository](https://github.com/antonio-morales/Fuzzing101/tree/main). Accessed: [09/06/2023].

[2] Andrea Fioraldi, Dominik Maier, Heiko Eißfeldt, and Marc Heuse. “AFL++: Combining incremental steps of fuzzing research”. In 14th USENIX Workshop on Offensive Technologies (WOOT 20). USENIX Association, AFL++ [Online]. Available: [AFL++](https://github.com/AFLplusplus/AFLplusplus). Aug. 2020. Accessed: [09/06/2023].

[3] [Exercise 1](https://github.com/antonio-morales/Fuzzing101/tree/main/Exercise%201) - Antonio Morales, "Fuzzing101" repository, Accessed [09/06/2023].

## Contact
