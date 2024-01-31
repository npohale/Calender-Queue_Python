# Calendar Queue (CQ) Documentation

The Calendar Queue (CQ) is a data structure modeled after a simple desk calendar, designed for efficient scheduling and management of events based on their priorities. This documentation provides an overview of the CQ structure, its key components, operations, and changes made to a pre-existing simulator (Simulus) for integration.

## Overview

The Calendar Queue operates on the concept of a desk calendar where events are scheduled by writing them on specific pages corresponding to each day. The priority of an event is determined by its scheduled time. In the computer implementation, each day's events are represented by a sorted linked list. The complete calendar consists of an array of pointers, with each pointer pointing to a linked list of events for a particular day.

## Key Components

### 1. Width and Resizing Policy

- **Width:** The time interval spanned by each bucket in the calendar. It is variable and depends on the number of events in each bucket.
- **Resizing:** The number of buckets is dynamically adjusted based on the number of events in the queue. Resizing occurs when the number of events exceeds or falls below specific thresholds.
- **Power of Two Rule:** The number of buckets is often chosen as a power of two (Nb = 2^k) for efficient resizing operations.

### 2. Index

- **Index:** A number assigned to a bucket using a hash function. Calculated as `(int(time/width)) % n_buckets`.

### 3. Other Attributes

- **Bucket Top:** Priority at the top of the bucket from which the last element was dequeued.
- **Lastbucket:** Bucket index from which the last element was dequeued.
- **LastPrio:** Priority of the last-dequeued event.
- **lastBucket:** Bucket number from which the last element was dequeued.

## Time Complexity Analysis

### 1. Insertion Operation

- The bucket number for an event is determined using a hash formula, resulting in an average time complexity of O(1).

### 2. Pop Item Operation

- Removal of the event with the least priority. At worst, requires going through all buckets, leading to O(N) time complexity.

### 3. Delete Item Operation

- Deleting a specific item from the calendar queue. Time complexity is O(m), where m is the number of elements in the bucket.

### 4. Update Item Operation

- Updating an item involves checking each bucket for the event with the correct ID, resulting in O(N) time complexity.

### 5. Resize Operations

- Creating a new calendar queue with a different size and width, copying all current elements. Time complexity depends on the resizing conditions.

## Changes Made to Pre-Existing Simulator

1. **Event.py Changes:**
   - Removed the pre-existing PQDict class and replaced it with the Calendar Queue Class.
   - Basic operations of insertion, deletion, dequeuing, and updating events are implemented.
   - Resizing operations are added, crucial for the data structure.

2. **Simulator.py Changes:**
   - Modified inner code for handling a list of lists as the base storage of events.
   - The purpose and output of the `Show Calendar` method remain the same.

## Running the Modified Simulator

1. Unzip the file in the location of your choice, preferably Desktop or C Drive.
2. Run the command `pip install simulus`.
3. Locate the imported simulus in the Site Packages folder.
4. Replace the original `event.py` and `simulator.py` with the modified versions from the zip file.
5. Run the examples with the modified plugins and cross-check the results with the provided output files.

## References

- [NetDB Calendar Scheduler Documentation](https://netdb.cis.upenn.edu/rapidnet/doxygen/html/classns3_1_1_calendar_scheduler.html#_details)
- [NS-3 Calendar Scheduler Source Code](https://www.nsnam.org/docs/release/3.18/doxygen/calendar-scheduler_8cc_source.html)
- Randy Brown, "Calendar Queues: A Fast O(1) Priority Queue Implementation for the Simulation Event Set Problem," CACM, 31(10):1220-1227, October 1988.
