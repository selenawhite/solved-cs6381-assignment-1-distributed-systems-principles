Download Link: https://assignmentchef.com/product/solved-cs6381-assignment-1-distributed-systems-principles
<br>
<strong><u>Single Broker-Based Publish-Subscribe Using ZMQ and Mininet</u></strong>

In this assignment we will build upon the PUB/SUB model supported by the ZeroMQ (ZMQ) middleware. One of the downsides of ZMQ’s approach is that there is no anonymity between publishers and subscribers. A subscriber needs to know where the publisher is (i.e., subscriber must explicitly connect to a publisher using its IP address and port). So, we lose some degree of decoupling with such an approach (recall the time, space, and synchronization decoupling that we studied). A more desirable solution is where application logic of the publishers and subscribers remains anonymous to each other; naturally something else will still need to maintain the association. This entity is the pub-sub middleware, which is the focus of this assignment.

<table>

 <tbody>

  <tr>

   <td width="228"></td>

  </tr>

  <tr>

   <td></td>

   <td></td>

  </tr>

 </tbody>

</table>

In this assignment, we will build a small additional layer of pub/sub middleware on top of existing ZMQ capabilities to support anonymity. See the diagram below of the expected abstraction.




Feel free to use the underlying capabilities of ZMQ that you feel appropriate. For instance, the REQ-REP and PUB-SUB are the most likely features of ZMQ you will need. No matter what, the ZMQ operations should be hidden from your publisher and subscriber application logic. The application code should use the APIs of your middleware, which then translate into ZMQ calls under the hood.

We will maintain a broker that we assume is going to be well known to the publishers and subscribers. It is this broker that then does the matchmaking. Once the match is made, you have two choices as to how data should be disseminated from publishers to subscribers. Write your code in such a way that the approach is globally configurable across the system to use one of the two approaches when the system is deployed.

<ul>

 <li>Have the publisher’s middleware layer directly send the data to the subscribers who are interested in the topic being published by this publisher. This means that, as the number of subscribers for this topic increases, the publisher has to send data to each of the subscribers.</li>

</ul>

OR

<ul>

 <li>Have the publisher’s middleware always send the information to the broker, which then sends it to the subscribers for this topic. Such an approach saves the amount of data sent by the publisher but clearly can make the broker a bottleneck as the number of publishers and subscribers in the system increases.</li>

</ul>

Ensure that your code is designed with extensibility in mind because subsequent assignments are going to enhance this basic design. <strong>We will use Python3 for our assignments.</strong>

After writing the code for the assignment, you should be able to conduct a variety of end-to-end performance measurement experiments to get a sense of the impact on amount of data sent, latency of dissemination, pinpointing the source of bottlenecks, etc. Automation in this testing process is preferred so that you do not have to manually set up each experiment.

<strong><u>API Specification</u></strong>

One might consider an API that your middleware provides as described below.

First, both publishers and subscribers will require an API to talk to the broker, for which we might use something like this:

<strong>Publisher uses this to register itself with the broker for a given topic it publishes.</strong>

register_pub (topic = &lt;some string&gt;, &lt;some identification of the publisher&gt;)




<strong>Subscriber uses this to register itself with the broker for the topic it is interested in.</strong>




register_sub (topic = &lt;some string&gt;, &lt;some identification of the subscriber&gt;)




Since a publisher application may be publishing different kinds of topics, it will use the following API to publish its publication:

publish (topic = &lt;string&gt;, value = &lt;val&gt;)

Subscriber application can have a notify method, which is a push-based method as follows:

notify (topic = &lt;string&gt;, value = &lt;val&gt;)




<strong>Use of Github and Grading Criteria:</strong>

We will be peer grading the assignments. Each team will be assigned a team number. A numbered team will grade the team number one higher than theirs modulo number of teams (so last team grades team 1). <strong>Thus, T1 grades T2; T2 grades T3; T3 grades T4; T4 grades T5; T5 grades T6; and T6 grades T1.</strong> Proper unit tests must be created to test various features. The unit tests should be supplied by a team to their peer grading team. The peer grading team must evaluate whether the supplied unit tests will test all (maximal) number of features.

I suggest all teams use github for their code base. Please share your CS 6381-specific contributions with me in addition to your grading team. <strong>Your instructor will provide his/her Github handle during a live session.</strong>

<strong>Assignment-Specific Demonstration:</strong>

<ul>

 <li>Should work with all the properties mentioned in the assignment.</li>

 <li>Should work with multiple publishers publishing multiple different publications and multiple subscribers, all of them distributed over different hosts over different kinds of network topologies that we can create in Mininet (instructor may provide their topologies).</li>

 <li>Subscribers may join any time and leave any time.</li>

</ul>

Do end-to-end measurements (time between publication and receipt of information; since the clock is the same on all emulated hosts, we do not have the issue of clocks drifting apart from each other). Plot graphs and have them saved in the repo that you maintain