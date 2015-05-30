---
title: Python script to find synapse voltage peaks and other parametric data from Cadence Virtuoso log file
layout: default
---
<br>

<h4>{{ page.title }}</h4>
  


<p class="muted">{{ page.date | date_to_long_string }}</p>
<br><br>

This script takes the log file from spectre as an input and analyses it to find where the peak(both negative and positive) occurs for all the sets of data with different design parameters. It outputs the time of occurence of minimum peak, time of occurence of maximum negative peak, neuro-concen, re-uptake, maximum positive voltage, maximum negative voltage in the form of a list with data in the sequence as: 

<b>[time of occurence of max. positive peak, time of max negative peak, neuro-conc., re-uptake, max. postive peak voltage, max. negative peak voltage] 
</b>

The script:<br><br> 


<pre>"""
 Created on Fri Jun 21 11:26:00 2013
 Description: This script takes the log file from spectre as an input and
        analyses it to find where the peak(both negative and positive)
        occurs for all the sets of data with different design parameters.
        It outputs the time of occurence of minimum peak, time of
        occurence of maximum negative peak, neuro-concen, re-uptake,
        maximum positive voltage, maximum negative voltage in the form of
        a list with data in the above sequence.
 @author: siddharth
 """
 #declaring and initialising all the variables used
 fp = open("siddharth.log","r")
 buff = []
 temp = [0,0,0,0,0,0]
 final = []
 array1 = []
 count = 0
 max_value = 0
 min_value = 0
 max_time = 0
 min_time = 0
 zero_check = 0# becomes 0 when data starts, else 1
 def func(buff):# function to find the max v and time and min v and time
   global max_value
   global max_time
   global min_value
   global min_time
   if buff[1] > max_value:
     max_value = buff[1]
     max_time = buff[0]
   elif buff[1] < min_value:
     min_value = buff[1]
     min_time = buff[0]
   return  
 def convert(buff): #function to convert string to numbers
   buff[0] = float(buff[0])
   buff[1] = float(buff[1])      
 for line in fp:#iterating each line, storing in a buffer, splitting and analysing
   if line == '\n':#removing all empty spaces from the log file
     continue  
   buff = line.split() #splits the line into a list of strings
   if buff[0] == 'neuro_conc':#to extract neuro-conc
     temp[2] = float(buff[2])
   if buff[0] == 'R' :  #to extract re-uptake
     temp[3] = float(buff[2])
   if buff[0] == 'time': # change zero_check to 1 if no data, only text
     zero_check = 1
     print "the max value is:"
     print max_value
     temp[4] = max_value
     print "the max time is:"
     print max_time
     temp[0] = max_time
     print "the min value is:"
     print min_value
     temp[5] = min_value
     print "the min time is:"
     print min_time    
     temp[1] = min_time
     if max_time != 0: #to ignore initial values with all zero entries
       final.append(temp)
     temp = [0,0,0,0,0,0]
     min_value = 0#re-initialising for each data set
     max_time = 0
     max_value = 0
     min_time = 0
     abc=raw_input("press enter to start new block")
   if buff[0] =='0.00e+00':#data starts whenever u see this
     abc = raw_input("press enter to start data values")    
     print "data starts"
     zero_check = 0#flag to indicate data started
   if zero_check == 0:#convert string to numbers if data is there
     convert(buff)    
     func(buff)
   print buff
 print zero_check  
 fp.close()  
 for line in final:
   print line</pre>