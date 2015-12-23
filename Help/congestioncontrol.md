![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)

|---------------------------------------------|
| ![](spacer.gif)UDT based congestion control |

The UDT Congestion Control Algorithm
====================================

Yunhong Gu

Aug. 2009

OVERVIEW
========

UDT uses an AIMD (additive increase multiplicative decrease) style congestion control algorithm. The algorithm controls the sending rate by tuning the packet sending period (i.e. how often to send out one packet). This period is presented by the symbol **SND** in the rest of this document. The sending rate is increased every 10 milliseconds according to Section 3 if and only if at least one acknowledgement (ACK) has been received since the last rate increase. The rate will be decreased according to Section 4 upon receiving a packet loss report (negative acknowledgement, or NAK). The rate may or may not be decreased on a timeout event, see Section 5.

Note that the packet sending period is different from the inter-packet time. The former includes the time that is required to send out a packet, which can be significant in high speed networks. The latter does not include the packet sending time.

UDT also uses a supportive window-based control by tuning the maximum number of on-flight packets (unacknowledged packets) (**CWND**). An upper threshold of CWND should be used and set as large enough to not limit throughput. A lower threshold should be set as well to avoid deadlock (CWND cannot be 0). In UDT, the lower threshold is 2. The CWND value is updated when an ACK is received.

The congestion control includes two phases: it starts with a slow start phase and then enters then regular phase as described above. The slow start phase runs exactly once at the beginning of a connection and stops when a packet loss occurs or CWND reaches its maximum value.

**Implementation Note 1.1**: Initially, SND is set at 0 and CWND is set at 2.

**Implementation Note 1.2**: When necessary, the packet sending interval does not need to be strictly enforced. The sender can sends out a group of packets and then sleeps an aggregate amount of inter-packet time. For example, if SND = 10ms, the sender can sends out 10 packets continuously and sleep 100ms minus the time used to send out those 10 packets. This is especially important as most operating systems have a minimum sleep time (e.g., 10ms).

**Implementation Note 1.3**: The window control is independent of flow control. However, they can be enforced at the same place in the sender. The sender should stop sending once the number of the on-flight packets reaches either the congestion window size or the flow window size, until a new ACK comes.

PRELIMINARY
===========

ASSUMPTIONS
-----------

UDT assumes most of packets have the same size. If this assumption does not hold, the unit “packets per second” in the rest of the paper should be converted to bits per second.

UDT increases its sending rate every 10 milliseconds, this constant time is defined as **SYN**. At least one ACK should be sent back every SYN, provided that there are data packets received.

Packet Pair Technique
---------------------

UDT requires an estimated value of the link capacity. This is done by using a packet pair technique. In this technique, the sender sends two back-to-back packets of the same size. Once these two packets arrive at the receiver side, there will be an interval between the two packets. The link capacity can then be determined by

B = packet size / interval

UDT sends out a packet pair every 16 packets. However, when it happens that there is no 17th packet to be sent out, UDT will still send out the 16th packet, rather than waiting for the next one. In this case, there will be a bigger interval (hence underestimation) at the receiver side. The receiver should use a median filter to detect and discard such values.

In addition, other patterns of packet pairs can be used, as long as the receiver has a way to identify the packet pairs.

**Implementation Note 2.1**: All packets in the packet pairs should have the same size. The ideal packet size used in packet pair should be the path MTU size (including all packet headers). If this is not possible, the size should be as close to the MTU size as possible. However, the size must not be bigger than MTU.

Because UDT was designed for transferring big data, there should be always enough MTU-sized packets to use. In certain situations where most packets are small ones, those packets can still be used for packet pair estimation, although overestimation may be caused, depending on how small the packets are. In addition, if the packets are sent out very frequently, they can be combined into bigger ones. If the packets are sent less frequently, then they are not likely to cause any congestion (hence the congestion control may not be necessarily strictly enforced).

If the sizes of the packets are too small or the two packets have different sizes, this packet pair should be ignored. The receiver keeps a window of the last *N* packet pairs. The value *N* can be any proper number as long as it can reach a reasonable estimation, and it should not be too large to consume too much memory and CPU time. In UDT, *N* = 64.

Here the data structure “window” is a circular linked list with a fixed size of *N*. However, it does not matter how it is implemented, as long as it can store a list of historical values and uses the most recent value to replace the oldest value each time.

The receiver should use a median filter to filter out noises. UDT computes a median value of the intervals kept in the window. All values greater than 8x of the median or less than 1/8 of the median should be ignored. UDT then computes the average of the rest intervals. The link capacity will then be calculated using the average interval.

B = packet size / average\_interval

Note that this formula is based on the assumption that most packets have the same size. If this assumption does not hold, B should be computed from each pair independently and then the average B is calculated.

The window should be filled with very large intervals (i.e., low bandwidth, for safety purpose) initially. UDT uses 1 second.

The estimation is done at the receiver side. The receiver should send this value back to the sender. This can be carried by any proper control packets. UDT carries the estimation value in every ACK.

The sender, upon receiving a new value, should use a smooth average to update its own value:

B = B \* 0.875 + b \* 0.125

where B is the link capacity value that the sender maintains and b is the value that is just sent back by the receiver.

**Note 2.1:** Accuracy of the packet pair technique may depend on the network equipments and network congestion. Both overestimation and underestimation can happen. However, the rate increase formula in Section 3 can tolerate misestimation to a scale up to 10x or 1/10 of the real link capacity.

**Implementation Note 2.2**: The link capacity value B can be sent only with selected ACKs (e.g., every 10 ACKs or every 10ms). It depends on how often an ACK is generated. Calculating B is time consuming, so some ACKs can be skipped if ACKs are sent very often. This principle applies to the packet arrival rate (AS) as well, see Section 2.4.

RTT and RTO
-----------

UDT needs to update RTT (Round Trip Time) and RTO (Retransmission Time Out). UDT sends one ACK2 (acknowledgement of acknowledgement) for selective ACK. The receiver records the time when an ACK is sent out. The ACK carries a unique sequence number (independent of the data packet sequence number). The corresponding ACK2 also carries the same sequence number. This technique is called ACK sub-sequencing.

Upon receiving the ACK2, UDT calculates the RTT by comparing the difference between the ACK2 arrival time and the ACK departure time. In the following formula, “RTT” is the current value that the receiver maintains and “rtt” is the recent value that was just calculated from ACK/ACK2.

*RTT = RTT \* 0.875 + rtt \* 0.125*

*RTTVar = RTTVar \* 0.875 + abs(RTT - rtt)) \* 0.125*

The RTT value should be sent back to the sender, e.g., in ACK. UDT does not transmit RTTVar to the peer side. The peer side will update its own RTT and RTTVar values using the same formulae as above, in which case “rtt” is the most recent value it receives, e.g., carried by an incoming ACK.

Both RTT and RTTVar are changing variables. RTT is not a fixed value. It can be changed by congestion or even route change. RTTVar records the variance of the RTT value and is required to compute RTO:

*RTO = RTT + 4 \* RTTVar*

**Note 2.2:** A UDT socket can both send and receive. RTT, RTTVar, and RTO are not just sender specific variables. They are shared by both the sender and the receiver (of the same socket). When a UDT socket receives data, it updates its local RTT and RTTVar, which can be used for its own sender as well.

**Implementation Note 2.3**: There is no need to calculate RTT and RTTVar in the same way as UDT does. If a protocol acknowledges immediately after receiving a data packet, RTT can be calculated by the difference between the ACK arrival time and the departure time of the corresponding data packet. In this case, there is no need to send ACK2.

RTO is the time that an acknowledgement is expected after a data packet is sent out. If there is no acknowledgement after this amount of time elapsed, a timeout event should be triggered.

Since UDT only acknowledges every SYN time, in UDT

*RTO = RTT + 4 \* RTTVar + SYN*

**Implementation Note 2.4:** It is important to have a lower threshold for RTO. If RTO is too small, retransmission can be easily triggered by a busy CPU, etc. UDT uses 100ms as the lower threshold.

**Implementation Note 2.5:** Continuous timeout should increase the RTO value. In UDT, a counter (ExpCount) is used to track the number of continuous timeout.

*RTO = ExpCount \* (RTT + 4 \* RTTVar) + SYN*

Packet Arrival Rate
-------------------

The receiver records the intervals between any two adjacent data packet arrivals. The number of interval values can be limited by a window size. The packet arrival rate can be calculated by

AS = 1 / average\_interval

The average packet arrival interval also requires a media filter, similar to that in Section 2.2. The thresholds are the same as those used for B (link capacity): 8x for upper threshold and 1/8 for lower threshold.

**Implementation Note 2.6:** Similar to the estimated bandwidth, if most of the data packets are less than MTU, the arrival rate can be computed as bits per second, rather than number of packets per second.

WHEN AN ACK COMES
=================

**This event and only this event can trigger the increase of a packet sending rate (or reduce packet sending period SND) and the updated of the size of the congestion window (CWND).**

The congestion control is in the slow start phase initially. Within the slow start phase, the packet sending period (SND) is 0. The window size starts at 2 and is set to the number of total sent packets so far every time an ACK comes.

Slow start phase stops upon receiving an NAK or the maximum size of the window has been reached. CWND has an upper threshold (“maximum size of the window”). This upper threshold is used for safety purpose (even if there is no packet loss, the slow start phase has to stop at a certain point). The threshold can be set to the maximum of the receiver buffer size.

At the end of the slow start phase, the packet interval should be set to the reverse of the packet arrival rate:

SND = 1/ AS

There is nothing else to do during the slow start phase. The slow start phase will only run exact once at the beginning of a connection.

In the regular phase (post slow start), the rate increase can be defined according to the following formulae:

First, the number of sent packets to be increased in the next SYN period (inc) is calculated as:

*if (B \<= C)*

*inc = 1/MSS;*

*else*

*inc = max(10^(ceil(log10((B-C)\*MSS\*8))) \* Beta/MSS, 1/MSS);*

where B is the estimated link capacity sent back by the receiver (Section 2.2) and C is the current sending speed (C = 1/SND) . Both are counted as packets per second. MSS is the fixed/maximum size of UDT packet counted in bytes, usually MSS = MTU – UDP/UDT packet header size. Beta is a constant value of 0.0000015.

**Note 3.1**: If B and C use bits per second as the unit, “MSS” should be removed from the (B-C)\*MSS part. This can be helpful if most of the packets are less than MTU size.

The SND value is the updated as:

*SND = (SND \* SYN) / (SND \* inc + SYN).*

Meanwhile, the congestion window size should be updated by:

*CWND = AS \* (RTT + SYN) + 16*

where AS is the packet arrival rate sent back by the receiver (Section 2.4).

**During the regular phase, sending rate cannot be increased more than once during any SYN time.** Meanwhile, although CWND can be updated for all ACKs, it is usually updated together with SND.

WHEN AN NAK COMES
=================

Congestion Period
-----------------

Every time an NAK is received, UDT updates the largest sequence number has been sent so far (LastDecSeq). We define a congestion period as the time between two NAKs in which the biggest lost packet sequence number carried in the NAK is greater than the LastDecSeq.

For example, if the sender has sent out packets 1 – 100 and an NAK comes informing the sender that packet 60 is missing, the LastDecSeq value should be updated as 100. If after a while, another NAKs comes indicating that packet 140 is lost, this NAK starts a new congestion period, which lasts until the next period starts.

Note that one congestion period can cause multiple packet loss events. For example, if the sender sends out packet 1 – 100, packets 60 and 80 are lost. When the sender receives NAK(60), a new congestion period is started, and the sender updates LastDecSeq as 100. When the sender receives NAK(80), which is less than LastDecSeq(100), so the sender knows that it already decreased the sending rate before (at NAK-60) and it may decrease less for this packet loss (UDT uses a random process to decide how much to decrease).

After the sender decreases the sending rate at NAK(60) or possibly NAK(80), it continues to send out new packets 101, 102, etc. For example, when it receives an NAK(140), which is greater than LastDecSeq(100), this means that the current sending rate is still too high: packets sent after a rate decrease still got dropped.

Decreasing Sending Rate
-----------------------

These four parameters are used in rate decrease, and their initial values are in the parentheses: AvgNAKNum (1), NAKCount (1), DecCount(0), LastDecSeq (initial sequence number - 1).

AvgNAKNum is the average number of NAKs during a congestion period. NAKCount is the number of NAKs received so far in the current congestion period. DecCount means the number of times that the sending rate has been decreased during the congestion period.

*If it is in the slow start phase, set SND to 1/AS. Slow start ends. Stop.*

*If this NAK starts a new congestion period*

*{*

*increase SND = SND \* 1.125;*

*Update AvgNAKNum;*

*Reset NAKCount to 1;*

*Compute DecRandom to a random (uniform distribution) number between 1 and AvgNAKNum;*

*Reset DecCount to 1;*

*Reset Update LastDecSeq;*

*Stop.*

*}*

*If (DecCount \<= 5) and (NAKCount == DecCount \* DecRandom)*

*{*

*Update SND period: SND = SND \* 1.125;*

*Increase DecCount by 1;*

*}*

*Update LastDecSeq and NAKCount.*

UDT decreases the sending rate for each congestion period by a random factor between 1/8 to 1/2. For the first NAK in the congestion period, UDT decreases its sending rate by 1/8 (SND = SND \* 1.125). For the following NAKs, UDT uses a random number (DecRandom) to decide if the rate should be decreased or not. DecRandom is a random number between 1 and the average number of NAKs per congestion period (AvgNAKNum).

For example, if DecRandom = 2, then UDT will decrease its sending rate on the 1st, 2nd, 4th, 6th, … NAKs. However, the sending rate should not be decreased by more than 1/2 in each congestion period, so UDT will stop decreasing after the 6th decrease (DecCount \<= 5), as 1.125^6 = 2.

RETRANSMISSION
==============

The expiration of the RTO timer can trigger the transmission of packets sending and may decrease of the sending rate as well. Note that RTO is NOT set per packet/message; it is a system timer and it is reset by every ACK and NAK.

In UDT 4.4, the sending rate is unchanged. In UDT 4.5, the sending rate is halved (SND = SND \* 2). The latter is recommended now because it is safer for low bandwidth networks.

Because RTO is actually greater than RTT (Section 2.3), there should not be more than one timeout per RTT.

Finally, after the aggregate continuous timeouts exceeds a threshold, the connection should be deemed to be broken.

|-------------------------|
| ![](spacer.gif)See Also |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="index.html">Index</a><br /></p></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><br /></p></td>
</tr>
</tbody>
</table>
