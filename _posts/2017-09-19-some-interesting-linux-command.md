---
id: 4472
title: some interesting linux command
date: 2017-09-19T07:31:06+00:00
author: admin
layout: post
guid: http://blog.ikow.cn/?p=4472
permalink: /some-interesting-linux-command/
categories:
  - linux
tags:
  - command
  - interesting command
  - linux
  - shell
---
#### 1. Supervise command (run every 2s)

<div class="highlight">
  <pre>watch <span class="s2">"ls -larth"</span>
</pre>
</div>

#### 2. Kill program using one port

<div class="highlight">
  <pre>sudo fuser -k 8000/tcp
</pre>
</div>

#### 3. Limit memory usage for following commands

<div class="highlight">
  <pre><span class="nb">ulimit</span> -Sv <span class="m">1000</span>       <span class="c1"># 1000 KBs = 1 MB</span>
<span class="nb">ulimit</span> -Sv unlimited  <span class="c1"># Remove limit</span>
</pre>
</div>

#### 4. Rename selected files using a regular expression

<div class="highlight">
  <pre>rename <span class="s1">'s/\.bak$/.txt/'</span> *.bak
</pre>
</div>

#### 5. Get full path of file

<div class="highlight">
  <pre>readlink -f file.txt
</pre>
</div>

#### 6. List contents of tar.gz and extract only one file

<div class="highlight">
  <pre>tar tf file.tgz
tar xf file.tgz filename
</pre>
</div>

#### 7. List files by size

<div class="highlight">
  <pre>ls -lS
</pre>
</div>

#### 8. Nice trace route

<div class="highlight">
  <pre>mtr google.com
</pre>
</div>

#### 9. Find files tips

<div class="highlight">
  <pre>find . -size 20c             <span class="c1"># By file size (20 bytes)</span>
find . -name <span class="s2">"*.gz"</span> -delete  <span class="c1"># Delete files</span>
find . -exec <span class="nb">echo</span> <span class="o">{}</span> <span class="se">\;</span>      <span class="c1"># One file by line</span>
./file1
./file2
./file3
find . -exec <span class="nb">echo</span> <span class="o">{}</span> <span class="se">\+</span>      <span class="c1"># All in the same line</span>
./file1 ./file2 ./file3
</pre>
</div>

#### 10. Print text _ad infinitum_

<div class="highlight">
  <pre>yes
yes hello
</pre>
</div>

#### 11. Who is logged in?

<div class="highlight">
  <pre>w
</pre>
</div>

#### 12. Prepend line number

<div class="highlight">
  <pre>ls <span class="p">|</span> nl
</pre>
</div>

#### 13. Grep with Perl like syntax (allows chars like \t)

<div class="highlight">
  <pre>grep -P <span class="s2">"\t"</span>
</pre>
</div>

#### 14. Cat backwards (starting from the end)

<div class="highlight">
  <pre>tac file
</pre>
</div>

#### 15. Check permissions of each directory to a file

It is useful to detect permissions errors, for example when configuring a web server.

<div class="highlight">
  <pre>namei -l /path/to/file.txt
</pre>
</div>

#### 16. Run command every time a file is modified

<div class="highlight">
  <pre><span class="k">while</span> inotifywait -e close_write document.tex
<span class="k">do</span>
    make
<span class="k">done</span>
</pre>
</div>

#### 17. Copy to clipboard

<div class="highlight">
  <pre>cat file.txt <span class="p">|</span> xclip -selection clipboard
</pre>
</div>

#### 18. Spell and grammar check in Latex

<div class="highlight">
  <pre>detex file.tex <span class="p">|</span> diction -bs
</pre>
</div>

You may need to install the following: `sudo apt-get install diction texlive-extra-utils`.

#### 19. Check resources&#8217; usage of command

<div class="highlight">
  <pre>/usr/bin/time -v ls
</pre>
</div>

#### 20. Randomize lines in file

<div class="highlight">
  <pre>cat file.txt <span class="p">|</span> sort -R
cat file.txt <span class="p">|</span> sort -R <span class="p">|</span> head  <span class="c1"># Pick a random sambple</span>

<span class="c1"># Even better (suggested by xearl in Hacker news):</span>
shuf file.txt
</pre>
</div>

#### 21. Keep program running after leaving SSH session

If the program doesn&#8217;t need any interaction:

<div class="highlight">
  <pre>nohup ./script.sh <span class="p">&</span>
</pre>
</div>

If you need to enter some input manually and then want to leave:

<div class="highlight">
  <pre>./script.sh
&lt;Type any input you want&gt;
&lt;Ctrl-Z&gt;          <span class="c1"># send process to sleep</span>
<span class="nb">jobs</span> -l           <span class="c1"># find out the job id</span>
<span class="nb">disown</span> -h jobid   <span class="c1"># disown job</span>
<span class="nb">bg</span>                <span class="c1"># continue running in the background</span>
</pre>
</div>

Of course, you can also use `screen` or `tmux` for this purpose.

#### 22. Run a command for a limited time

<div class="highlight">
  <pre>timeout 10s ./script.sh

<span class="c1"># Restart every 30 minutes</span>
<span class="k">while</span> true<span class="p">;</span> <span class="k">do</span> timeout 30m ./script.sh<span class="p">;</span> <span class="k">done</span>
</pre>
</div>

#### 23. Combine lines from two sorted files

<div class="highlight">
  <pre>comm file1 file2
</pre>
</div>

Prints these three columns:

  1. Lines unique to `file1`.
  2. Lines unique to `file2`.
  3. Lines both in `file1` and `file2`.

With options `-1, -2, -3`, you can remove each of these columns.

#### 24. Split long file in files with same number of lines

<div class="highlight">
  <pre>split -l LINES -d file.txt output_prefix
</pre>
</div>

#### 25. Flush swap partition

If a program eats too much memory, the swap can get filled with the rest of the memory and when you go back to normal, everything is slow. Just restart the swap partition to fix it:

<div class="highlight">
  <pre>sudo swapoff -a
sudo swapon -a
</pre>
</div>

#### 26. Fix ext4 file system with problems with its superblock

<div class="highlight">
  <pre>sudo fsck.ext4 -f -y /dev/sda1
sudo fsck.ext4 -v /dev/sda1
sudo mke2fs -n /dev/sda1
sudo e2fsck -n &lt;first block number of previous list&gt; /dev/sda1
</pre>
</div>

#### 27. Create empty file of given size

<div class="highlight">
  <pre>fallocate -l 1G test.img
</pre>
</div>

#### 28. Manipulate PDFs from the command line

To join, shuffle, select, etc. `pdftk` is a great tool:

<div class="highlight">
  <pre>pdftk *.pdf cat output all.pdf        <span class="c1"># Join PDFs together</span>
pdftk <span class="nv">A</span><span class="o">=</span>in.pdf cat A5 output out.pdf  <span class="c1"># Extract page from PDF</span>
</pre>
</div>

You can also manipulate the content with `cpdf`:

<div class="highlight">
  <pre>cpdf -draft in.pdf -o out.pdf      <span class="c1"># Remove images</span>
cpdf -blacktext in.pdf -o out.pdf  <span class="c1"># Convert all text to black color</span>
</pre>
</div>

#### 29. Monitor the progress in terms of generated output

<div class="highlight">
  <pre><span class="c1"># Write random data, encode it in base64 and monitor how fast it</span>
<span class="c1"># is being sent to /dev/null</span>
cat /dev/urandom <span class="p">|</span> base64 <span class="p">|</span> pv -lbri2 &gt; /dev/null

<span class="c1"># pv options:</span>
<span class="c1">#   -l,  lines</span>
<span class="c1">#   -b,  total counter</span>
<span class="c1">#   -r,  show rate</span>
<span class="c1">#   -i2, refresh every 2 seconds</span>
</pre>
</div>

#### 30. Find packages that have a given file in Ubuntu

<div class="highlight">
  <pre>apt-file update
apt-file search dir/file.h</pre>
</div>