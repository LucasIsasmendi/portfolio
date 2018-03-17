# JSDev Talks Node

## Core
### Interprocess Communication in NodeJS
[Marko Jurisic - viennaJS - February, 2017] (https://pusher.com/sessions/meetup/viennajs/interprocess-communication-in-nodejs)

Node is single threaded and uses non-blocking event loops for IO but for CPU intensive tasks it still blocks. If we want to use more CPU cores to parallelize the processing we must create new processes and distribute the load. This talk describes our initial problem and a few iterations until we found a passable solution.
