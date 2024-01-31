##Calendar Queue (CQ)


It is a structure modelled after a simple desk calendar. One schedules an event on a desk
calendar by simply writing it on the appropriate page, one page for each day. There may be
several events scheduled on a particular day, or there may be none. The time at which an
event is scheduled is its priority. The enqueue operation corresponds to scheduling an
event. The earliest event on the calendar is dequeued by scanning the page for today’s date
and removing the earliest event written on that page.

In the computer implementation, each page of the calendar is represented by a sorted
linked list of the events scheduled for that day. An array containing one pointer for each
day of the year is used to find the linked list for a particular day. If the array name is
“bucket,” then bucket[12] is a pointer to the list of events scheduled for the 12th day of the
year. The complete calendar thus consists of an array of 365 pointers and a collection of up
to 365 linked lists.

In our case, each event has an associated timestamp (or time). This time is what decides
priority of occurrence of events, i.e. lower the time, higher the priority.
A bucket (an array of events in our case) has some associated attributes:

###1. Width and Resizing Policy
 It is the time interval between spanned by that bucket, e.g. a simple desk calendar
has 365 buckets each having a width of one day.
 The width of a bucket is variable and it depends on the number of events in each
bucket. The buckets are resized up or down according to certain conditions. If the
number of events in the queue is much smaller or much larger than the number of
buckets, it will not function efficiently. The solution is to allow the number of
buckets to correspondingly grow and shrink as the queue grows and shrinks. To
simplify the resize operation, the Nb (number of buckets) in a CQ is often chosen
to be the power of two, i.e. Nb = 2
 The number of buckets is doubled or halved each time the Ne (number of events)
exceeds 2Nb or decreases below Nb/2 respectively. When Nb is resized, the new
width w has to be calculated as well. The new w that is adopted will be estimated
by sampling the average inter-event time gap from the first few hundred events
starting at the current bucket position. Thereafter, a new CQ is created and all the
events in the old calendar will be recopied over.
 The length of a day (bucket width) should he adjusted each time the queue is
copied onto another calendar. If the bucket width is much greater than the
average separation of adjacent queue elements, the queue elements will be
clustered in a small number of buckets near the current date and the rest will he
empty. If the bucket width is too small, most queue elements will not be in the
current year.
 Enqueue and dequeue times are smallest when the bucket width is somewhere in
the vicinity of the average separation of adjacent queue elements. Since the
separation of adjacent queue elements generally decreases as more elements are
added to the queue, the bucket width should he readjusted when the queue is
copied to another calendar.

 Buckets containing events about to be dequeued normally contain the most
queue elements, since they have not been emptied for a full year. An easy way to
calculate the new bucket width is to dequeue a few elements, calculate the
average separation of the dequeued events, and then put them back on the
queue.
2. Index: It is a number assigned to a bucket to differentiate it from other buckets. We
use a Hash function to assign the bucket index to an event. It is calculated as follows
(n_buckets = total number of buckets in the CQ):
3. Index = (int(time/width) )% n_buckets
4. Bucket Top: Priority at the top of the bucket from which the last element was
dequeued.
5. Lastbucket : Bucket index from which the last element was dequeued
6. LastPrio: Lastprio is the priority of Event which is just dequeued from the Calendar
Queue
7. lastBucket: lastBucket is the bucket number from which last element is dequeued
Time Complexity Analysis

 Insertion Operation
The bucket number the event will go is basically dependant on the time at which
event is scheduled. It is found out by simple hash formula
Index= ((time of event)/width of bucket) %(Number of Buckets)
This will get the event to desired bucket where it is inserted. Almost always the width
of the buckets and the number of the buckets will resize themselves such that
average time complexity will result in O(1)
 Pop Item Operation
This operation basically is removal of the event with least priority. This operation
demands knowing elements at the top of each bucket. This will at worst will demand
to go through all buckets of the Bucket array leading to O(N) time complexity
 Delete Item
This operation is deleting a specific given item from the calendar queue. This
operation will effectively reach the specified bucket using time of the event to be
deleted and will remove the event once it has reached the bucket with correct
hashing. Since this operation goes through each and every element of the bucket it
will cost O(m) time complexity in which m is number of elements in the bucket
 Update Item

This operation goes to each and every bucket and checks for the event with correct
id to be updated this will cost O(N) time complexity.
 Resize Operations
These operations will make a completely new calendar queue and of new size and
width and will copy all the current elements into new calendar queue.

###Changes Made to Pre-Existing Simulus

1)To Event.py
 I have removed the pre-existing PQDict class and replaced it with my Calendar
Queue Class
 I have used basic operation of insertion deletion dequeuing and updating
events but removed the sink and swim from the modified code
 I have used resizing operations which are actually crucial for the data structure.
The number of buckets should always be greater than half the number of
elements and width should be in multiple of 3s
 I have also modified event list eliminating almost all usage of event ids
 In event list i have kept name and purpose of all events same just change inner
code of them to be compatible with Calendar Queue Class
2)Simulator.py
Since implementation involves list of lists as base storage of the events, we cannot
access them as Dict-Values as done in earlier version so just modified inner code but
kept the purpose and output same of the Show Calendar Method
Method to run the modified Simulus DES
1. Unzip the file in location of your choice. Preferably Desktop or C Drive
2. Run the pip install simulus Command
3. Find your imported simulus imported in Site Packages folder in C:/Users/(Your User
Name) / AppData(This is hidden Folder)/Local/Programs/Python/Python310/site-
packages/simulus
4. To run the original Simulus just run the examples in the zip file
5. Note this is the by default simulus on which your command prompt will run the
simulus examples so in order to use my plugins first back up the original event.py and
simulator.py and then replace those with those in the zip file.
6. Now run the examples with my modified plugins and cross check the results with the
output files also given along in the zip file.
Ref:
https://netdb.cis.upenn.edu/rapidnet/doxygen/html/classns3_1_1_calendar_scheduler.html#_details
 https://www.nsnam.org/docs/release/3.18/doxygen/calendar-scheduler_8cc_source.html
 Randy Brown, Calendar Queues: A Fast 0(1) Priority Queue Implementation for the
Simulation Event Set Problem, CACM, 31(10):1220-1227, October 1988.
