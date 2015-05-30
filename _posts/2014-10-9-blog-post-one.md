---
title: Python script to execute java files
layout: default
---
<br>

<h4>{{ page.title }}</h4>
  


<p class="muted">{{ page.date | date_to_long_string }}</p>
<br><br>

To execute java files through python, we execute a bash script called shellcmds.sh that contains bash commands to execute a java(.jar) file. 
the python script:
<br><br>
<pre>import subprocess  
 subprocess.call("./shellcmds.sh",shell=True)  </pre>

The shell script contains commands to first go into the dist/ folder which contains the java script and then execute it. 
the shell script: 
<br><br>
<pre>cd dist  
 java -jar PythonScriptRunner.jar  </pre>