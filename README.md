## FAQ 
### What's this?
**Fulgance** is a text-based railroad simulator that will let you perform various tasks:
- define realistic or custom (fancy) routes (without ramps and curves for now)
- define schedules
- define rolling stock (only EMUs for now, next on the list are steam engines)
- distribute schedules over mutiple cores if you have many trains (only single process so single core for now)
- run schedules in real-time or accelerated time on the default route or your own routes (only accelerated-time for now)
- jump into a train to check its real-time progress (the jumpseat is not implemented yet)
- calibrate trains frequency at peak and off peak hours (not yet implemented)
- calculate power consumption (not yet implemented, depends on ramps and curves)
- get lots of statistics for data crunching and rendering

###Notes for route designers
####Layout
- routes may use on-track signals or electronic signals (only on-track for now) for trains separation
- Fulgance will run the default route located in default/ 
- Routes are made of segments located in (routeName)/segments/(segmentName)/. For example: default/segments/WestboundMain/ for the default route
- A segment is a set of contiguous blocks where each block is under control of a signal
- Blocks are not explicitely delimited or managed in Fulgance. In fact, signals define blocks.
- Blocks are unidirectional, except the ones protected by a **reversing signal** 
- In each segment, you need to describe the location of signals (SIGs), stations (STAs) and speed limits (TIVs). 
- All route and schedule data are kept in simple, self-explanatory text files. Lines beginning with a hash are ignored, as one would expect.
####Signalling guide
The following on-track signals are or will be implemented:
- "Type 1": this is the usual 3-aspect signal. The possible states are: VL (green, all clear), A (yellow, prepare to stop at next signal) and C (red, impassable stop).
- "Type 2A": this is both a buffer (i.e a segment termination) signal and a reversing signal. Its only possible state is C (red, impassable).
- "Type 2B": this is a buffer signal, a reversing signal and a diverging/converging junction signal. It allows reversing to a segment which is different from the origin segment. Its only possible state is C (red, impassable).
- "Type 3": this is a single aspect signal, its state is always A (yellow). Type 3 must always precede a type 2.
- "Type 4D": this is a diverging junction signal, used to stitch segments together.
- "Type 4C": this is a converging junction signal, used to stitch segments together.
Junction signals can manage only two legs, not more: one to the main segment (left or right), the other one to the diverging/converging segment (left or right).They must have a unique name in both segments, so that the engine may stich segments properly.

### Requirements
#### Single and multi core environnement
- Python 2.7
- A redis server running on the localhost (apt-get install redis-server in Ubuntu). Redis is used for signal aspects real-time management.
#### Multi core environment
- taskset (part of utils-linux in Ubuntu).
Note: in multi-core environments, redis is also used for inter-process synchronisation.
#### Jumpseat
- Python curses (the jumpseat will not work on Windows, sorry...)
Note: jumpseat relies on redis for getting the real-time state of a train.

### Let's get started
- To run the engine in single-core mode, simply run mp.py without arguments
- Schedules are located in default/schedules/
- The engine (mp.py) will start the default schedule of the default route located in default/schedules/default.txt
