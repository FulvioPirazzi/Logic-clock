# Logic-clock
a simple led  time keeper

1.1  ####The exact specification of the project submitted and approved by the professor.####
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
