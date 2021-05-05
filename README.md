# Logic-clock
a simple led  time keeper

#### 1.1 The exact specification of the project submitted and approved by the professor.####
To develop an implementation that takes a signal with a certain regular frequency as input. This logic signal
is used as a time reference in order to provide the time in two formats:
1. hh:mm:ss.
2. Clock’s dial made using aligned LED as hands of seconds, minutes, and hours.
The aim is to implement a clock, and if possible with chronometer functions (reset, start, stop).
1.2
Supplementary details about the specification of the project.
The signal mentioned is an alternated logic value between two values like zero and one. Obviously the two
values can represent true or false, but the idea is to have a referential clock in order to synchronize the
implementation. From now we will refer to that clock as ”signal” in order to avoid confusion, for example,
with the clock’s dial.
The frequency of the signal is supposed to be as near as possible approximated to one Hertz, because the
smaller period’s resolution will be a second.
The hh:mm:ss.format can also be called digital, and the 3 values will be showed as decimal. The clock’s dial
format can be also called analogue, and again we will have 3 values represented by the three hands (seconds,
minutes, hours). Since the LED implementation provide a low graphic resolution, it will be necessary in
some way, to avoid confusion between the three hands. The chronometer functions are intended as feature
to be developed:
Reset is intended as the ability of regulating in order to achieve accuracy and synchronization with the
time.
Start and stop will provide the ability to suspend the constant incrementation of the time. Combining
this feature with the regulation, it will be possible to start from a certain time.




2
2.1
Analysis specification and implementation choices.
Identifications of the required inputs.
First class of inputs are the various conditions set by the user in order to perform the features like start, stop
and regulate the time. A different type of input is the signal module. Even if it is not provided by the user,
we can consider it as an input data since it needs to be manipulated.
2.2
The signal module as input.
The signal module is an infinite series of alternate zero and one at well defined frequency. In order to drive
the two clock representations we need to manipulate the signal obtaining two informations, one for each
representation.
2.3
Start, stop and regulation of the time as input.
Important parts of the project are the start-stop and regulation of time displayed features. The start-stop
function can be implemented as switches signal (0 or 1) that will interact with the system. The start-stop
switch, at certain state, will interrupt the signal module: For example if start-stop is 0, the signal module
will be always zero, and this can be obtained using an AND (∧) logic gate as in table 1.


A B A∧B
0 0  0
0 1  0
1 0  0
1 1  1

Table 1: Let assign start-stop=A, signal module=B, result=A∧B

Concerning the regulation of time, the normal running time is implemented using cyclic shift registers,
meaning that at each step the last value goes to the first position, and in that way we provide the cyclic
nature of the time representation. In order to gain an extra step, we have to provide an extra impulse in the
clock, which can be achieved using an OR (∨) logic gate as in table 2.

A B A∨B
0 0  0
0 1  1
1 0  1
1 1  1

Table 2: Let assign increment=A, original clock=B, resulted clock=A∨B

2.4 Identifications of the required output.
The two representations of the time, analogue and digital can be assumed as output or as final result.

2.5
The digital clock as output.
The digital clock needs basically three information: Hour, minute, and second. Hours have a cycle of 12
increment steps, and an increment of the hours occurs every complete cycle of the minutes. Minutes have 60
incremental steps, and an increment of the minutes occurs at every complete cycle of the seconds. Seconds
have 60 incremental step, and an incremental step of the seconds takes place every edge or complete cycle of
the signal module, about every second, since the signal will be at frequency of one Hertz.
Tkgate natively provides a seven-segment displays, that emulates Seven-segment Liquid Crystal Display
(LCD). Three couple of them will perfectly perform the digital clock. Each of the couple requires the input
value expressed in binary.
42.6
The analog clock as output.
The analog clock requires again three information, but here, instead of a number, we need a position. A
good implementation of this position can be an array of boolean values related to each possible position. The
hand can assume only a position at the time, thus only one of the values will be set as 1 (true) at each step.
We will need three of these set positions, hours, minutes and second. At each step (of the signal module)
the position of the second will change. The trigger mechanism between the three set positions is intuitive
and very similar to the digital one. For the seconds and minutes we need 60 boolean values and for hours 12
boolean values are needed. It is also important to notice that by leaving the last position the set cycle will
start again, providing a clock signal for the higher set. As for the digital clock, we have the same behaviour
which is a periodic repetition of the values between 0 and 11 or 0 and 59.
2.7
Relationships between input and output.
Having defined the inputs and outputs allow us to proceed by evaluating which input and output attributes
can be exploited. In order to proceed, it is worth to notice that there are basically two strategies and by
choosing one or the other will significantly change the project implementation:
The first strategy is to solve the two problems (digital and analog) in parallel, by starting both from the
signal module and obtaining the result separately.
The second strategy uses the solution of one to achieve the second.
I decided to adopt the second solution, solving first the analog issue then the digital one. The analog part
can be implemented by using shift registers. As the name suggests a shift register shifts the values on the
register at each step, which exactly implements our change of position. The digital part can be gained from
the analog one by using a decoder which basically converts a number with a single 1 in position x into a
binary number y.
The first task of the implementation is to get the three integers which drive the analog clock. In other
words we have to produce a number x of n digit (n = 12 for hour and n=60 for seconds). The binary
number x will be composed by 0 except for a unique 1 that represents the position. Having x representing
the position it is useful because we can then directly drive the analog clock using each single digit to drive
the correspondent position. The number x needs to be updated at each period of the signal module. As
already stated, it can be easily done by using shift registers.
Concerning the digital clock, as we mentioned before, we need to use a decoder that translates the position
x in the binary number representing the time. To be precise, we have three decoders for the numbers x1
(seconds), x2 (minutes), x3 (hours), but the concept of the three decoder is the same, and we will investigate
in detail only once. The decoder mentioned before will take an integer x as input and it will produce another
integer y as output. Those two integers (x, y) are related by the relation log x = y that can be also expressed
as 2 y = x as explained in the table 3.


value  Description
0      Logic 0 or false
1      Logic 1 or true
x      Unknown state (could be 0, 1 or z)
z      Floating state
L      High unknown state (could be 1 or z)
L      Low unknown state (could be 0 or z)
Table 4: [1] http://www.tkgate.org/2.0/gateHDL.html # wiretypes

In the project there are some critical parts that produce the Unknown state. As an example the value on
the shift register at the beginning are x. When a logic gate receives x as input, the result is in general the x
value, in other word the unwanted value propagates. Note that by default a LED will assume a third color
(Yellow) in case of Unknown state. In order to avoid this problem there are mainly two solutions:
The first solution sets the same color of the LED for logic 0 and unknown state, thus hiding the problem
at the final user, but the behaviour of the gates remains a problem. This solution is partially permanent
because the simulator settings are saved locally. The second solution, that we adopt, consists in running a
sort of start-up sequence, providing the right value along all the parts of circuits.



