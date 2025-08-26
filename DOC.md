## Events / Actions Table (Agent)

| **Event**                       | **Action(s) triggered**                                                               |
| ------------------------------- | ------------------------------------------------------------------------------------- |
| `initializeE(ID, N, Preferred)` | Reset votes, set `agent_id`, `total_agents`, `preferred_value`, compute `max_rounds`. |
| `show_preferredE`               | Display current `preferred_value`.                                                    |
| `show_idE`                      | Display `agent_id`.                                                                   |
| `show_roundE`                   | Display `round`.                                                                      |
| `show_tallyE`                   | Display `tally`.                                                                      |
| `show_votesA` (action)          | Print list of `received_votes`.                                                       |
| `start_consensusE`              | Calls `send_voteA`.                                                                   |
| `send_voteA` (precondition)     | Allowed only if `round ≤ max_rounds`.                                                 |
| `send_voteA` (action)           | Broadcast `vote(ID, Preferred)` to all agents.                                        |
| `voteE(SenderID, Preferred)`    | Add `(SenderID, Preferred)` to `received_votes`.                                      |
| `update_majorityE`              | Show received votes, compute majority → update `preferred_value`, reset votes.        |
| `king_broadcastE`               | Calls `king_broadcast_action` + `king_broadcast_actionA`.                             |
| `king_broadcast_action` (check) | True if `agent_id = round`.                                                           |
| `king_broadcast_actionA`        | If king, broadcast `king_vote(ID, Preferred)`.                                        |
| `king_voteE(KID, KingValue)`    | If precondition holds, call `king_vote_updateA`.                                      |
| `king_vote_precondition`        | Check quorum: `tally < (N/2 + N/4 + 1)` and agent not king.                           |
| `king_vote_updateA(KingValue)`  | Replace `preferred_value` with king’s value.                                          |
| `nextroundE`                    | Reset votes, increment round, reset tally.                                            |

---

## Simulator-Level Events (agentRoundSimulator)

| **Simulator Event**    | **Action(s) triggered**                                                   |
| ---------------------- | ------------------------------------------------------------------------- |
| `initializeE`          | Sends initialization to agents (ID, N, Preferred), then show preferences. |
| `firstbroadcastE`      | Broadcast `start_consensus` to all agents.                                |
| `firstupdatemajorityE` | Broadcast `update_majority` to all agents.                                |
| `kingbroadcastE`       | Broadcast `king_broadcast` to all agents.                                 |
| `nextroundE`           | Broadcast `show_preferred` + `nextround` to all agents.                   |

---

## Collaboration Diagram
The collaboration diagram essentially forms a clique, which reflects the nature of the problem domain. In general network topologies of distributed agents, achieving consensus (or electing a leader) is not always possible. However, in this algorithm, every agent communicates with all others (or can communicate in the Byzantine case). Because of this design, the algorithm does not require complex FIPA directives—simple message sending is sufficient for exchanging values.
