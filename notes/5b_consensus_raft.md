# Consensus: Raft Algorithm TODO

The Raft algorithm can be composed into two sections, namely 
1. Leader Election
2. Log Replication (accept changes from clients, propagate new log)

## How it Works

- Followers 
  - Listen for AppendEntries
  - Grant votes to candidates
  - Election Timeout
- Candidate
  - Becoming a candidate
    - If follower's election timeout elapses
    - Become candidate
    - Increment term
    - Vote for self
    - Reset Election Timer
  - Send RequestVote RPCs to all servers
    - If enough votes become leader
    - If an AppendEntries RPC is obtained from a valid leader, step down
    - Election Timeout Occurs --> Start new election term
    - Discover higher term --> Step Down
- Leader (TODO)
  - Send initial empty AppendEntries as heartbeat
  - Accept commands from clients
    - Appends command to log
    - Execute command in state machine and return result to client
    - Leader notifies followers of committed entries in AppendEntries
    - Followers execute committed commands in their respective state machine
  - What to do if followers/crashed
    - Retry AppendEntries RPC until they succeed
  - Ideally one RPC

## Node States

State | Type |Explanation
---|---|---
Follower | Passive Participant | Only gives regular heartbeats
Candidate | Active Participant| Issues RequestVote RPCs to get elected to leader
Leader | Active Participant | Issues AppendEntries RPCs to replicate its log

Standard operation is `1 Leader : N-1 Followers`

## Terms

::: define Term (Raft)
Time period, whereby there is at most 1 leader (sometimes none)
:::

- Each Server maintains a current term value
  - Sent via every RPC
  - Peer has later term --> Update term and become follower
  - Peer has obsolete term --> Reply with error

## Heartbeats and Timeouts

- Servers start as followers
- Leaders must send heartbeats
- If an electionTimeout elapses without any RPCs (e.g. Heartbeat)
  - Followers assume leader crash
  - New election

::: question What do Raft Heartbeats contain
Empty AppendEntries RPCs
:::

## Election 

- Candidate increments current term
- Vote for self
- Send requestVote to other servers
- If...
  - No one wins and timeout elapses
    - Begin again (incl. increment)
  - Receive RPC from valid leader
    - Become Follower
  - Receive majority votes
    - Become leader
    - Send Heartbeat

This allows...
- safety, as at most one winner per term
- liveness, as some candidate must eventually win
  - due to election timeouts being randomly selected (between `[T, 2T]`, whereby T could be 150ms)
  - one server will timeout, meaning the other is likely to win

---

::: question Election Timeout Typical Duration
100-500ms
:::