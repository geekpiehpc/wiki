# fuck numa in linux but fine in epyc

```bash
Every 2.0s: numastat                                                                epyc.node1: Mon Aug 30 07:17:40 2021

                           node0           node1
numa_hit             11605557098     17090418391
numa_miss                      0               0
numa_foreign                   0               0
interleave_hit             83929           83526
local_node           11605248266     17089868634
other_node                308832          549757
```