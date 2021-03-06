# Distributed Eventual Queue for Vortex OpenSplice
In many situations (at least those we can imagine right now) such an distributed-eventual-queue is used in a 'worker-pattern' where you can load-balance the work around multiple queue-readers.
Rather than load-balancing on individual samples, this version load-balances over instances (assuming that samples of an instance form a 'work-package').

# Getting Started
* compile the distributed queue broker (dqbroker.c) requires Vortex OpenSplice
* start it with 2 parameters and 1 optional: dqbroker [-1] <partition> <topic>
  * Default is instance-basis yet with a '-1' parameter it schedules 'per sample'
  * where <partition> is the name of the 'queue-partition' that this broker will 'handle'
  * where <topic> is the name/pattern of the topics to be 'brokered'
  * where <topic> published in <partition> are brokered towards mathing <topic> readers that each utilize a dedicate partition
* the above test/image was generated using:
  * brokering circles in the default partition i.e.: dqbroker "" Circle 
  * publishing Circles in the default partition (and creating a local reader in that same partition with history-depth 30)
  * starting 3 ishapes readers (each with history depth 10)
    * demo_ishapes "banaan"
    * demo_ishapes "banaan1"
    * demo_ishapes "banaan2"

## Some Notes
* the broker creates reader/writers 'on the fly' and uses the QoS of the 'endpoints'
* the broker uses synchronous reliability towards the readers 
* its up to the set of readers to choose a unique partition
* when multiple readers choose the same partition, they will receive the same data-set (could be useful for other patterns)
* the broker uses KEEP_ALL with history-limits '10' for its generated reader/writers

# Vortex Overview
PrismTech’s Vortex Intelligent Data Sharing Platform provides the leading implementations of the Object Management Group’s Data Distribution Service (DDS) for Real-time Systems standard. DDS is a middleware protocol and API standard for data-centric connectivity and is the only standard able to meet the advanced requirements of the Internet of Things (IoT). DDS provides the low-latency data connectivity, extreme reliability and scalability that business and mission-critical IoT applications need. For more information visit www.prismtech.com/vortex .

# Support
This is a proof of concept/prototype/alpha and is therefore provided as is with no formal support. If you experience an issue or have a question we'll do our best to answer it. In order to help us improve our innovations we would like your feedback and suggestions. Please submit an issue and/or provide suggestions via the GitHub issue tracker or by emailing innovation@prismtech.com.

# License
All use of this source code is subject to the Apache License, Version 2.0. http://www.apache.org/licenses/LICENSE-2.0

“This software is provided as is and for use with PrismTech products only.

DUE TO THE LIMITED NATURE OF THE LICENSE GRANTED, WHICH IS FOR THE LICENSEE’S USE OF THE SOFTWARE ONLY, THE LICENSOR DOES NOT WARRANT TO THE LICENSEE THAT THE SOFTWARE IS FREE FROM FAULTS OR DEFECTS OR THAT THE SOFTWARE WILL MEET LICENSEE’S REQUIREMENTS.  THE LICENSOR SHALL HAVE NO LIABILITY WHATSOEVER FOR ANY ERRORS OR DEFECTS THEREIN.  ACCORDINGLY, THE LICENSEE SHALL USE THE SOFTWARE AT ITS OWN RISK AND IN NO EVENT SHALL THE LICENSOR BE LIABLE TO THE LICENSEE FOR ANY LOSS OR DAMAGE OF ANY KIND (EXCEPT PERSONAL INJURY) OR INABILITY TO USE THE SOFTWARE OR FROM FAULTS OR DEFECTS IN THE SOFTWARE WHETHER CAUSED BY NEGLIGENCE OR OTHERWISE.

IN NO EVENT WHATSOEVER WILL LICENSOR BE LIABLE FOR ANY INDIRECT OR CONSEQUENTIAL LOSS (INCLUDING WITHOUT LIMITATION, LOSS OF USE; DATA; INFORMATION; BUSINESS; PRODUCTION OR GOODWILL), EXEMPLARY OR INCIDENTAL DAMAGES, LOST PROFITS OR OTHER SPECIAL OR PUNITIVE DAMAGES WHATSOEVER, WHETHER IN CONTRACT, TORT, (INCLUDING NEGLIGENCE, STRICT LIABILITY AND ALL OTHERS), WARRANTY, INDEMNITY OR UNDER STATUTE, EVEN IF LICENSOR HAS BEEN ADVISED OF THE LIKLIHOOD OF SAME.

ANY CONDITION, REPRESENTATION OR WARRANTY WHICH MIGHT OTHERWISE BE IMPLIED OR INCORPORATED WITHIN THIS LICENSE BY REASON OF STATUTE OR COMMON LAW OR OTHERWISE, INCLUDING WITHOUT LIMITATION THE IMPLIED WARRANTIES OF MERCHANTABLE OR SATISFACTORY QUALITY AND FITNESS FOR A PARTICULAR PURPOSE, TITLE AND NON INFRINGEMENT ARE HEREBY EXPRESSLY EXCLUDED TO THE FULLEST EXTENT PERMITTED BY LAW. “
