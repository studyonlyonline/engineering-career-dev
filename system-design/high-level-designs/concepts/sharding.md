### Key points
- We need sharding when you hit the ceiling of what a single machine can handle
- Bring about sharding only when you are talking about scaling.
- We require sharding
- 

### Sharding strategies
- Range based sharding
- Hash based Sharding
  - Always go with consistent hashing sharding technique.
- Directory based sharding (use in celebrity hotspot problem)
  - There is lookup based. Lookup based means you save in which shard what data is present

### Issues with Sharding
- Hotspot problem (One shard has high load)
- Cross shard operations
  - Now we have to lookup across multiple shards to get data
- Maintaining consistency

### Whenever talking about sharding
- Propose a shard key based on access pattern
- Choose your distribution strategy (Consistent Hashing technique)
- Call out the tradeoff (global queries which do no match access pattern)
- Address how we will handle the growth

NOTE: WE DO NOT ALWAYS NEED TO SHARD BECAUSE MODERN HARDWARE IS STRONG
Example - Postgress 140TB and 50K Reads

### Reading material
- <https://www.hellointerview.com/learn/system-design/core-concepts/sharding>
- [Hello interview](https://www.youtube.com/watch?v=L521gizea4s&t=410s)
