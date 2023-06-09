# Honggfuzz+
## Getting started


## Building and installing HonggFuzz+

### AFL++ installation

We will be utilizing the most recent version of the AFL++ fuzzer for this course.

Install the required dependencies by executing the following commands:
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

Checkout and build AFL++

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


Assuming the previous steps were successful, you should now be able to run afl-fuzz. Simply type:
<div>
  <pre>
    <code class="language-bash">
    afl-fuzz
    </code>
  </pre>
</div>
This should display something similar to the following output.


![image](https://github.com/sbamohabbatchafjiri/Honggfuzzplus/assets/47651730/5eea3389-0401-4c07-afc9-bb6a48811754)







## Contact
