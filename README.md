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

![image](https://github.com/sbamohabbatchafjiri/Honggfuzzplus/assets/47651730/7b2d92a4-dae0-4af0-9185-78bce6ae414e)

### Download and build your target

As you can use differet target for fuzzing, we represent a general representation of we you need to do. To access more targets and more details about sample targets, please visit the [Fuzzing101](https://github.com/antonio-morales/Fuzzing101/tree/main) website created by Antonio Morales [1].
 
<div>
  <pre>
    <code class="language-bash">
      cd $HOME
      mkdir fuzzing_target && cd fuzzing_target/
    </code>
  </pre>
</div>

<div>
  <pre>
    <code class="language-bash">
      wget https://dl.from_official_web_page.com/target.tar.gz
      tar -xvzf target.tar.gz
    </code>
  </pre>
</div>

<div>
  <pre>
    <code class="language-bash">
      cd target
      sudo apt update && sudo apt install -y build-essential gcc
      ./configure --prefix="$HOME/fuzzing_target/install/"
      make
      make install
    </code>
  </pre>
</div>

<div>
  <pre>
    <code class="language-bash">
      cd $HOME/fuzzing_target
      mkdir target_file_samples && cd target_file_examples
      wget https://web_over_internet/sample1.file_format
      wget https://web_over_internet/sample2.file_format
      wget https://web_over_internet/sample3.file_format
      wget https://web_over_internet/sample4.file_format
    </code>
  </pre>
</div>
To ensure everything is working properly, you can test each library by running the respective command lines provided below. Although they may have slight variations, they follow a similar form:

For libexif:
<div>
  <pre>
    <code class="language-bash">
$HOME/fuzzing_libexif/install/bin/exif
    </code>
  </pre>
</div>
For Xpdf:

<div>
  <pre>
    <code class="language-bash">
$HOME/fuzzing_xpdf/install/bin/pdfinfo -box -meta $HOME/fuzzing_xpdf/pdf_examples/helloworld.pdf
    </code>
  </pre>
</div>
For TCPdump:
<div>
  <pre>
    <code class="language-bash">
$HOME/fuzzing_tcpdump/install/sbin/tcpdump -h
    </code>
  </pre>
</div>
For libTIFFF:

<div>
  <pre>
    <code class="language-bash">
$HOME/fuzzing_tiff/install/bin/tiffinfo -D -j -c -r -s -w $HOME/fuzzing_tiff/tiff-4.0.4/test/images/palette-1c-1b.tiff
    </code>
  </pre>
</div>

For libxml2:

<div>
  <pre>
    <code class="language-bash">
./xmllint --memory ./test/wml.xml
    </code>
  </pre>
</div>
By executing these command lines, you can test and verify that each library is functioning correctly. 


Here is an example from [Exercise 1](https://github.com/antonio-morales/Fuzzing101/tree/main/Exercise%201) created by Antonio Morales in the "Fuzzing101" repository [1]:

<div>
  <pre>
    <code class="language-bash">
      cd $HOME
      mkdir fuzzing_xpdf && cd fuzzing_xpdf/
    </code>
  </pre>
</div>

<div>
  <pre>
    <code class="language-bash">
      sudo apt install build-essential
    </code>
  </pre>
</div>

<div>
  <pre>
    <code class="language-bash">
      wget https://dl.xpdfreader.com/old/xpdf-3.02.tar.gz
      tar -xvzf xpdf-3.02.tar.gz
    </code>
  </pre>
</div>

<div>
  <pre>
    <code class="language-bash">
      cd xpdf-3.02
      sudo apt update && sudo apt install -y build-essential gcc
      ./configure --prefix="$HOME/fuzzing_xpdf/install/"
      make
      make install
    </code>
  </pre>
</div>

<div>
  <pre>
    <code class="language-bash">
      cd $HOME/fuzzing_xpdf
      mkdir pdf_examples && cd pdf_examples
      wget https://github.com/mozilla/pdf.js-sample-files/raw/master/helloworld.pdf
      wget http://www.africau.edu/images/default/sample.pdf
      wget https://www.melbpc.org.au/wp-content/uploads/2017/10/small-example-pdf-file.pdf
    </code>
  </pre>
</div>

<div>
  <pre>
    <code class="language-bash">
      $HOME/fuzzing_xpdf/install/bin/pdfinfo -box -meta $HOME/fuzzing_xpdf/pdf_examples/helloworld.pdf
    </code>
  </pre>
</div>

You will see: 

![image](https://github.com/sbamohabbatchafjiri/Honggfuzzplus/assets/47651730/e897a032-c185-4a16-a988-90e5c3afc029)


## Refrences
[1] Antonio Morales. Fuzzing101. [Online]. Available: [Fuzzing101 GitHub Repository](https://github.com/antonio-morales/Fuzzing101/tree/main). Accessed: [09/06/2023].

[2] [Exercise 1](https://github.com/antonio-morales/Fuzzing101/tree/main/Exercise%201) - Antonio Morales, "Fuzzing101" repository, Accessed [09/06/2023].

## Contact
