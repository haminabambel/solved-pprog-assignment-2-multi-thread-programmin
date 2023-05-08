Download Link: https://assignmentchef.com/product/solved-pprog-assignment-2-multi-thread-programmin
<br>
The purpose of this assignment is to familiarize yourself with Pthread and std::thread programming in C and C++, respectively. You will also gain experience measuring and reasoning about the performance of parallel programs (a challenging, but important, skill you will use throughout this class). This assignment involves only a small amount of programming, but a lot of analysis!

<hr>

<h2 id="1part1parallelcountingpiusingpthreads">1. Part 1: Parallel Counting PI Using Pthreads</h2>

<h3 id="11problemstatement">1.1 Problem Statement</h3>

This is a follow-up assignment from Assignment ZERO. You are asked to turn the serial program into a Pthreads program that uses a Monte Carlo method to estimate PI. The main thread should read in the total number of tosses and print the estimate. You may want to use <code>long long int</code>s for the number of hits in the circle and the number of tosses, since both may have to be very large to get a reasonable estimate of PI.

Your mission is to make the program as fast as possible. You may consider a lot of methods to improve the speed, such as SIMD intrinsics or a faster random number generator. However, you cannot break the following rules: you need to implement the Monte Carlo method using Pthreads.

Hint: You may want to use a reentrant and thread-safe random number generator.

You are allowed to use third-party libraries in this part, such as pseudorandom number generators or SIMD intrinsics.

<h3 id="12anamepart1_requirementsarequirements">1.2 <a name="part1_requirements"></a>Requirements</h3>

<ul>

 <li>Typing <code>make</code> in the <code>part1</code> directory should build the code and create an executable called <code>pi.out</code>.</li>

 <li><code>pi.out</code> takes two command-line arguments, which indicate the number of threads and the number of tosses, respectively. The value of the first and second arguments will not exceed the range of <code>int</code> and <code>long long int</code>, respectively. <code>pi.out</code> should work well for all legal inputs.</li>

 <li><code>pi.out</code> should output (to stdout) the estimated PI value, which is <strong>accurate to three decimal places</strong> (i.e., 3.141xxx) with at least <code>1e8</code> tosses.</li>

</ul>

Example:

<pre><code class="shell language-shell">$ make &amp;&amp; ./pi.out 8 10000000003.1415926....</code></pre>

<h2 id="2part2parallelfractalgenerationusingstdthread">2. Part 2: Parallel Fractal Generation Using std::thread</h2>

<h3 id="21problemstatement">2.1 Problem Statement</h3>

<pre><code class="shell language-shell">$ cd &lt;your_workplace&gt;$ wget https://nycu-sslab.github.io/PP-f21/HW2/HW2_part2.zip$ unzip HW2_part2.zip</code></pre>

Build and run the code in the <code>part2</code> directory of the code base. (Type <code>make</code> to build, and <code>./mandelbrot</code> to run it. <code>./mandelbrot --help</code> displays the usage information.)

This program produces the image file <code>mandelbrot-serial.ppm</code>, which is a visualization of a famous set of complex numbers called the Mandelbrot set. [Most platforms have a <code>.ppm</code> viewer. For example, to view the resulting images, use <a href="https://github.com/stefanhaustein/TerminalImageViewer.git" target="_blank" rel="noopener">tiv</a> command (already installed) to display them on the terminal.]

As you can see in the images below, the result is a familiar and beautiful fractal. Each pixel in the image corresponds to a value in the complex plane, and the brightness of each pixel is proportional to the computational cost of determining whether the value is contained in the Mandelbrot set. To get image 2, use the command option <code>--view 2</code>. (See function <code>mandelbrotSerial()</code> defined in <code>mandelbrotSerial.cpp</code>). You can learn more about the definition of the <a href="https://en.wikipedia.org/wiki/Mandelbrot_set" target="_blank" rel="noopener">Mandelbrot set</a>.

<img decoding="async" src="https://camo.githubusercontent.com/80f2e33b4e20f3f86809c6203402dc6807b389bc/687474703a2f2f67726170686963732e7374616e666f72642e6564752f636f75727365732f6373333438762d31382d77696e7465722f617373745f696d616765732f61737374312f6d616e64656c62726f745f76697a2e6a7067" alt="Mandelbrot Set">

Your job is to parallelize the computation of the images using <a href="https://en.cppreference.com/w/cpp/thread/thread" target="_blank" rel="noopener">std::thread</a>. Starter code that spawns one additional thread is provided in the function <code>mandelbrotThread()</code> located in <code>mandelbrotThread.cpp</code>. In this function, the main application thread creates another additional thread using the constructor <code>std::thread</code> (function, args…). It waits for this thread to complete by calling <code>join</code> on the thread object.

Currently the launched thread does not do any computation and returns immediately. You should add code to <code>workerThreadStart</code> function to accomplish this task. You will not need to make use of any other <code>std::thread</code> API calls in this assignment.

<h3 id="22requirements">2.2 Requirements</h3>

You will need to meet the following requirements and answer the questions (marked with “<strong>Q1-Q4</strong>“) in a <strong>REPORT</strong> using <a href="https://hackmd.io/" target="_blank" rel="noopener">HackMD</a>.

<ol>

 <li>Modify the starter code to parallelize the Mandelbrot generation using two processors. Specifically, compute the top half of the image in thread 0, and the bottom half of the image in thread 1. This type of problem decomposition is referred to as <strong><em>spatial decomposition</em></strong> since different spatial regions of the image are computed by different processors.</li>

 <li>Extend your code to use 2, 3, 4 threads, partitioning the image generation work accordingly (threads should get blocks of the image). <strong>Q1</strong>: In your write-up, produce a graph of <strong>speedup compared to the reference sequential implementation</strong> as a function of the number of threads used <strong>FOR VIEW 1</strong>. Is speedup linear in the number of threads used? In your writeup hypothesize why this is (or is not) the case? (You may also wish to produce a graph for VIEW 2 to help you come up with a good answer. Hint: take a careful look at the three-thread data-point.)</li>

 <li>To confirm (or disprove) your hypothesis, measure the amount of time each thread requires to complete its work by inserting timing code at the beginning and end of <code>workerThreadStart()</code>. <strong>Q2</strong>: How do your measurements explain the speedup graph you previously created?</li>

 <li>Modify the mapping of work to threads to achieve to improve speedup to at <strong>about 3-4x on both views</strong> of the Mandelbrot set (if you’re above 3.5x that’s fine, don’t sweat it). You may not use any synchronization between threads in your solution. We are expecting you to come up with a single work <strong><em>decomposition policy</em></strong> that will work well for all thread counts—hard coding a solution specific to each configuration is not allowed! (Hint: There is a very simple static assignment that will achieve this goal, and no communication/synchronization among threads is necessary.). <strong>Q3</strong>: In your write-up, describe your approach to parallelization and report the final 4-thread speedup obtained.</li>

 <li><strong>Q4</strong>: Now run your improved code with eight threads. Is performance noticeably greater than when running with four threads? Why or why not? (Notice that the workstation server provides 4 cores 4 threads.)</li>

</ol>