Download Link: https://assignmentchef.com/product/solved-cs2030s-project-1-discrete-event-simulator
<br>
<h3></h3>

<h4>Requirements</h4>

<ul>

 <li>OOP Principles and Design</li>

 <li>Use <tt>List</tt> and <tt>PriorityQueue</tt> from the <a href="https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/doc-files/coll-index.html" target="_blank" rel="noopener">Java Collections Framework</a> to store the servers and events;</li>

 <li>Packaging the classes into the package <tt>cs2030.simulator</tt>;</li>

 <li>Program style: adherance to <a href="https://www.comp.nus.edu.sg/~cs2030/style/" target="_blank" rel="noopener">CS2030 Java Style Guide</a></li>

 <li>Java documentation: adherence to <a href="https://www.comp.nus.edu.sg/~cs2030/javadoc" target="_blank" rel="noopener">CS2030 Javadoc Specification</a></li>

</ul>

<h4>Problem Description</h4>

A discrete event simulator is a software that simulates a system with events and states, and can be used to study many complex real-world systems such as queuing to order food at McDonald’s®. An event occurs at a particular time, and each event alters the state of the system and may generate more events. States remain unchanged between two events (hence the term <strong>discrete</strong>), and this allows the simulator to jump from the time of one event to another. A simulator typically also keeps track of some statistics to measure the performance of the system.

Let us simulate a queuing system comprising only one single customer queue and single service point <b>S</b> as shown below.

The system comprises the following:

<ul>

 <li>There is one <strong>server</strong> (a person providing service to the customer); the server can serve one customer at a time.</li>

 <li>The <strong>service time</strong> (time taken to serve a customer) is constant.</li>

 <li>When a <strong>customer</strong> arrives (<tt><strong>ARRIVE</strong></tt> event):

  <ul>

   <li>if the server is idle (not serving any customer), then the server starts serving the customer immediately (<tt><strong>SERVE</strong></tt> event).</li>

   <li>if the server is serving another customer, then the customer that just arrived waits in the queue (<tt><strong>WAIT</strong></tt> event).</li>

   <li>if the server is serving one customer, and another customer is waiting in the queue, and a third customer arrives, then the third customer leaves (<tt><strong>LEAVE</strong></tt> event). In other words, there is at most one customer waiting in the queue.</li>

   <li>When the server is done serving a customer (<tt><strong>DONE</strong></tt> event), the server can start serving the customer waiting at the front of the queue (if any).</li>

  </ul></li>

 <li>If there is no waiting customer, then the server becomes idle again.</li>

</ul>

There are five events in the system, namely <tt>ARRIVE</tt>, <tt>SERVE</tt>, <tt>WAIT</tt>, <tt>LEAVE</tt> and <tt>DONE</tt>. For each customer, these are the only possible event transitions:

<ul>

 <li><tt>ARRIVE</tt> → <tt>SERVE</tt> → <tt>DONE</tt></li>

 <li><tt>ARRIVE</tt> → <tt>WAIT</tt> → <tt>SERVE</tt> → <tt>DONE</tt></li>

 <li><tt>ARRIVE</tt> → <tt>LEAVE</tt></li>

</ul>

Statistics of the system that we need to keep track of are:

<ol>

 <li>the average waiting time for customers who have been served</li>

 <li>the number of customers served</li>

 <li>the number of customers who left without being served</li>

</ol>

As an example, given the arrival times (represented as a floating point value for simplicity) of three customers with one server, and assuming a constant service time of <tt>1.0</tt>,

<pre>0.5000.6000.700</pre>

the following events, each of the form <tt>&lt;time_event_occurred, customer_id, event_details&gt;</tt>, are produced

<pre>0.500 1 arrives0.500 1 served by 10.600 2 arrives0.600 2 waits to be served by 10.700 3 arrives0.700 3 leaves1.500 1 done serving by 11.500 2 served by 12.500 2 done serving by 1</pre>

with the end-of-simulation statistics being respectively, <tt>[0.450 2 1]</tt>.

<h4>Priority Queuing</h4>

The <tt>PriorityQueue</tt> (a mutable class) can be used to keep a collection of elements, where each element is given a certain priority.

<ul>

 <li>Elements may be added to the queue with the <tt>add(E e)</tt> method. The queue is modified;</li>

 <li>The <tt>poll()</tt> method may be used to retrieve and remove the element with the highest priority from the queue. It returns an object of type <tt>E</tt>, or <tt>null</tt> if the queue is empty. The queue is modified.</li>

</ul>

To enable <tt>PriorityQueue</tt> to order events, instantiate a <tt>PriorityQueue</tt> object using the constructor that takes in a <em>Comparator</em> object. <em>For more details, refer to the Java API Specifications.</em>

<h4>The Task</h4>

Given a set of customer arrival times in chronological order, output the discrete events. Also output the statistics at the end of the simulation.

Take note of the following assumptions:

<ul>

 <li>The format of the input is always correct;</li>

 <li>Output of a <tt>double</tt> value, say <tt>d</tt>, is to be formatted with <tt>String.format("%.3f", d)</tt>;</li>

</ul>

This task is divided into several levels. Read through all the levels to see how the different levels are related.

<strong>You have to complete ALL levels.</strong>

<table border="1" cellpadding="10">

 <tbody>

  <tr>

   <td><h4>Level 1</h4><big><strong>The Customer</strong></big>Write an <i>immutable</i> <tt>Customer</tt> class to represent each arriving customer that is tagged with a customer ID (of type <tt>int</tt>), as well as an arrival time (of type <tt>double</tt>). You may write other accessor methods as you deem necessary.<pre>jshell&gt; /open Customer.javajshell&gt; new Customer(1, 0.5)$.. ==&gt; 1 arrives at 0.5jshell&gt; new Customer(2, 0.6)$.. ==&gt; 2 arrives at 0.6jshell&gt; new Customer(3, 0.7)$.. ==&gt; 3 arrives at 0.7jshell&gt; /exit</pre></td>

  </tr>

  <tr>

   <td><h4>Level 2</h4><big><strong>The Server</strong></big>Write an <i>immutable</i> <tt>Server</tt> class to represent a server.<pre>class Server {    ...    Server(int identifier, boolean isAvailable, boolean hasWaitingCustomer, double nextAvailableTime) {        ...    }}</pre>The constructor of <tt>Server</tt> takes in four parameters:

    <ul>

     <li><tt>identifier</tt>: the identifier of the server</li>

     <li><tt>isAvailable</tt>: whether the server is currently available to serve;</li>

     <li><tt>hasWaitingCustomer</tt>: while server is serving, whether there is a waiting customer;</li>

     <li><tt>nextAvailableTime</tt>: while server is serving, the time for the current service to finish (that is the time when the server is able to serve the next customer)</li>

    </ul>There are only three possible types of servers to be generated for testing:

    <ul>

     <li><tt>new Server(i, true, <i>anything</i>, <i>anything</i>)</tt>: Server <tt>#i</tt> is available; a customer that arrives can be served immediately</li>

     <li><tt>new Server(i, false, false, t)</tt>: Server <tt>#i</tt> is busy, but with no waiting customer; i.e. it is available at time <tt>t</tt></li>

     <li><tt>new Server(i, false, true, t)</tt>: Server <tt>#i</tt> is busy, and with a waiting customer (to be served at time <tt>t</tt>)</li>

    </ul><pre>jshell&gt; /open Server.javajshell&gt; new Server(1, true, false, 0)$.. ==&gt; 1 is availablejshell&gt; new Server(2, false, false, 0.5)$.. ==&gt; 2 is busy; available at 0.500jshell&gt; new Server(3, false, true, 1.5)$.. ==&gt; 3 is busy; waiting customer to be served at 1.500jshell&gt; /exit</pre></td>

  </tr>

  <tr>

   <td><h4>Level 3</h4><big><strong>Events</strong></big>Till now we have written two independent <tt>Customer</tt> and <tt>Server</tt> classes. How do we link them together? Through events!Each event is an association between a customer and a set of servers. An event will also have the event start time, i.e. for <tt>ARRIVE</tt> events it will be the arrival time; for <tt>SERVE</tt> event it will be the time service starts; for <tt>DONE</tt> event, it will be the time the service ends, etc. All events are to be <i>immutable</i>.In this level, we shall test the event transitions for an arriving customer under different <tt>Server</tt> configurations. Each test starts off with an <tt>ArrivalEvent</tt> created using the constructor that takes in the arriving customer, and the list of servers. A transition from one event to the next happens whenever the <tt>execute</tt> method is called.<pre>$ jshell your_java_files_in_bottom-up_dependency_orderjshell&gt; new ArriveEvent(new Customer(1, 0.5), Arrays.asList(new Server(1, true, false, 0))) // server is available$.. ==&gt; 0.500 1 arrivesjshell&gt; new ArriveEvent(new Customer(1, 0.5), Arrays.asList(new Server(1, true, false, 0))).execute()$.. ==&gt; 0.500 1 served by 1jshell&gt; new ArriveEvent(new Customer(1, 0.5), Arrays.asList(new Server(1, true, false, 0))).execute().execute()$.. ==&gt; 1.500 1 done serving by 1jshell&gt; new ArriveEvent(new Customer(2, 0.6), Arrays.asList(new Server(1, false, false, 1.0))) // server is busy, but no waiting customer$.. ==&gt; 0.600 2 arrivesjshell&gt; new ArriveEvent(new Customer(2, 0.6), Arrays.asList(new Server(1, false, false, 1.0))).execute()$.. ==&gt; 0.600 2 waits to be served by 1jshell&gt; new ArriveEvent(new Customer(2, 0.6), Arrays.asList(new Server(1, false, false, 1.0))).execute().execute()$.. ==&gt; 1.000 2 served by 1jshell&gt; new ArriveEvent(new Customer(2, 0.6), Arrays.asList(new Server(1, false, false, 1.0))).execute().execute().execute()$.. ==&gt; 2.000 2 done serving by 1jshell&gt; new ArriveEvent(new Customer(3, 0.6), Arrays.asList(new Server(1, false, true, 1.0))) // server is busy with waiting customer$.. ==&gt; 0.600 3 arrivesjshell&gt; new ArriveEvent(new Customer(3, 0.6), Arrays.asList(new Server(1, false, true, 1.0))).execute()$.. ==&gt; 0.600 3 leavesjshell&gt; new ArriveEvent(new Customer(1, 0.5), Arrays.asList(new Server(1, false, false, 0), new Server(2, true, false, 0))).execute() // two servers: (1) busy; (2) free$.. ==&gt; 0.500 1 served by 2jshell&gt; new ArriveEvent(new Customer(1, 0.5), Arrays.asList(new Server(1, false, false, 0), new Server(2, true, false, 0))).execute().execute()$.. ==&gt; 1.500 1 done serving by 2jshell&gt; new ArriveEvent(new Customer(1, 0.5), Arrays.asList(new Server(1, false, true, 5.0), new Server(2, false, false, 10.0))).execute() // both servers busy, (1) has waiting customer$.. ==&gt; 0.500 1 waits to be served by 2jshell&gt; new ArriveEvent(new Customer(1, 0.5), Arrays.asList(new Server(1, false, true, 5.0), new Server(2, false, false, 10.0))).execute().execute()$.. ==&gt; 10.000 1 served by 2jshell&gt; new ArriveEvent(new Customer(1, 0.5), Arrays.asList(new Server(1, false, true, 5.0), new Server(2, false, false, 10.0))).execute().execute().execute()$.. ==&gt; 11.000 1 done serving by 2jshell&gt; new ArriveEvent(new Customer(1, 0.5), Arrays.asList(new Server(1, false, true, 5.0), new Server(2, false, true, 10.0))).execute() // both busy with waiting customer$.. ==&gt; 0.500 1 leavesjshell&gt; /exit</pre></td>

  </tr>

  <tr>

   <td><h4>Level 4</h4><big><strong>Scheduling Events</strong></big>We are finally ready to schedule events using the <tt>PriorityQueue</tt>.As an example, consider the start of a simulation with an initial schedule of three arrival events, and only one server.<pre>0.500 1 arrives0.600 2 arrives0.700 3 arrives</pre>The next event to pick is <tt>&lt;0.500 1 arrives&gt;</tt>. This schedules a <tt>SERVE</tt> event.<pre>0.500 1 served by 10.600 2 arrives0.700 3 arrives</pre>The next event to pick is <tt>&lt;0.500 1 served&gt;</tt>. This schedules a <tt>DONE</tt> event.<pre>0.600 2 arrives0.700 3 arrives1.500 1 done serving by 1</pre>The next event to pick is <tt>&lt;0.600 2 arrives&gt;</tt>…This process is repeated until there are no more events.Define a driver class <tt>Main.java</tt> that reads a series of arrival times, creates the arrival events, and starts the simulation. A skeleton class that simply reads the arrival times is given below:<pre>import java.util.Scanner;class Main {    public static void main(String[] args) {        Scanner sc = new Scanner(System.in);        int numOfServers = sc.nextInt();        while (sc.hasNextDouble()) {            double arrivalTime = sc.nextDouble();        }    }}</pre>The following is a sample run of the program. User input is underlined, and starts with an integer value representing the number of servers, followed by the arrival times of the customers. Input is terminated with a <tt>^D</tt> (CTRL-d).You will also need to compute the statistics (see problem description above) at the end of the simulation.


    <table border="1">

     <tbody>

      <tr>

       <td><tt>10.5000.6000.700^D0.500 1 arrives0.500 1 served by 10.600 2 arrives0.600 2 waits to be served by 10.700 3 arrives0.700 3 leaves1.500 1 done serving by 11.500 2 served by 12.500 2 done serving by 1[0.450 2 1]</tt></td>

      </tr>

      <tr>

       <td><tt>10.5000.6000.7001.5001.6001.700^D0.500 1 arrives0.500 1 served by 10.600 2 arrives0.600 2 waits to be served by 10.700 3 arrives0.700 3 leaves1.500 1 done serving by 11.500 2 served by 11.500 4 arrives1.500 4 waits to be served by 11.600 5 arrives1.600 5 leaves1.700 6 arrives1.700 6 leaves2.500 2 done serving by 12.500 4 served by 13.500 4 done serving by 1[0.633 3 3]</tt></td>

      </tr>

      <tr>

       <td><tt>20.5000.6000.7001.5001.6001.700^D0.500 1 arrives0.500 1 served by 10.600 2 arrives0.600 2 served by 20.700 3 arrives0.700 3 waits to be served by 11.500 1 done serving by 11.500 3 served by 11.500 4 arrives1.500 4 waits to be served by 11.600 2 done serving by 21.600 5 arrives1.600 5 served by 21.700 6 arrives1.700 6 waits to be served by 22.500 3 done serving by 12.500 4 served by 12.600 5 done serving by 22.600 6 served by 23.500 4 done serving by 13.600 6 done serving by 2[0.450 6 0]</tt></td>

      </tr>

     </tbody>

    </table>All classes dealing with the simulation should now reside in the <tt>cs2030.simulator</tt> package, with only the <tt>Main</tt> class importing the necessary classes from the package.Compile your code using<pre>javac -d . *.java</pre>and run your program to ensure that everything is still in good working order.</td>

  </tr>

 </tbody>

</table>

5/5 - (1 vote)