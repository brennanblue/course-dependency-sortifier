# course-dependency-sortifier
this program will take text files representing student choices and return courses sorted by semester into optimal order. The output is a 'critical path' showing the shortest route to the degrees/ programs specified by the user.  Outputting to a GANNT chart would be ideal, though that may wait for version 0.2.

This program does not yet exist, but it will run along the following lines. 

Input: serialized results of user choice form (http://webdev.uhh.hawaii.edu/timeline/). The format of this is malleable - if my python script lives on a webserver, the $_POST array would work, but for an intermediate step, the python app will assume local text files are available with user choice.

The program will be agnostic towards the number of items to sort.  Grouping loosely into semesters, we can expect a user pursuing a certificate program will have a shorter schedule than a user majoring in multiple four-year programs;

Certain details of the courses will be fetched, and stored in dictionaries.  Expect only the course name and number to provided from the intitial order: Prerequisites are stored in the UH Hilo course API (http://hilo.hawaii.edu/courses/api/1.1/help.txt), and will be referenced from there. 

Within the parser, the end product (shortest path / critical path) will be derived by workign backwards, from 4** level classes down to 1** level classes with no prerequisites.  
A blank template of 15 classes per semester will be created, and multiple semesters will be created as dependencies accrue. Nested arrays will likely be used to to acheive this end-  i.e. 

semesters[
{"name": "Final Semester",
"courses" :[
{"slot_1": "CS 453"},
{"slot 2": "CS 432"}
]
},
{"name": "2nd to last Semester",
"courses" : [
{"slot_1": "CS 352"}, //prereq for CS 453
{"slot_2": "CS 331"} // prereq fr CS 432
]}]

// ToDo - create sample data input format, so python development can be seperated from php 'menu' development (ongoing)

The iterator / parser should take an undifferentiated array of X items, and work through them in reverse order (array_flip?)
By keeping a running total of how many slots exist per semester (4 to 5), the code will know when a semester is 'full', and prompt / force overflow into a new semester slot. 

Some logic will need to be developed to prioritize overflow classes.  On the first pass, just adding new semesters when one is full should arrive at a 'passing' schedule (one where all prerequisites and grad requirements are met). There will likely be room for optimization of the schedule after this point, however. 

Another idea to be explored is to create a number of 'threads' based on individual classes, and their dependencies.  In this system, the most complex individual course might be a chain 6 links long, whilst gened requirements and simple courses are chains 1 link (semester) long.  Once the arrays of X_length chains are populated, the scheduling function would take these and fit them into the entire academic timeline, working from longest chains to the shortest.  

These ideas will be explored as psudocode / nonexistant code turns into something hackable. 
