# King Algorithm Implementation

## Introduction

The **King Algorithm** is a distributed consensus protocol designed to achieve agreement among multiple nodes, even in the presence of Byzantine faults. It proceeds in synchronous phases, where in each phase a designated "king" node helps resolve conflicts and guide the system toward consensus. This algorithm is particularly relevant in distributed systems that must tolerate faulty or malicious nodes.

Each node has a unique identifier and a preferred value. These values are initialized by the user at the start of the algorithm.

If there are fewer than

$$
f < \frac{n}{4}
$$

Byzantine nodes, then consensus is reached among all honest nodes within $f + 1$ phases. More generally, with the assumption above, consensus is guaranteed in

$$
\lceil \tfrac{n}{4} \rceil
$$

phases.

### Structure of a Phase

Each phase consists of **two rounds**:

1. **First Round:**

   * Each node broadcasts its preferred value to all other nodes.
   * Every node computes the majority value from the received messages.

2. **Second Round:**

   * The king node broadcasts its value to all others.
   * Each node adopts the king’s value *if* the majority value from the previous round was supported by fewer than $\frac{n}{2} + \frac{n}{4} + 1$

     votes.

---

## Installation (GNU/Linux)

1. Install **DALI** by following the guide in the [official repository](https://github.com/AAAI-DISIM-UnivAQ/DALI).
2. Clone this repository:

   ```bash
   git clone https://github.com/hurdersh/KingAlgorithm.git
   ```
3. Enter the project directory:

   ```bash
   cd KingAlgorithm/kingalgorithm
   ```
4. Start the project with:

   ```bash
   bash startmas_modular.sh
   ```

   or, if **tmux** is installed:

   ```bash
   bash startmas_tmux.sh
   ```

---

## Usage

Inside the directory `KingAlgorithm/kingalgorithm/mas/types`, you will find the following agent definitions:

* **agentHonest.txt** → an agent that follows the King Algorithm.
* **agentRoundSimulator.txt** → a simulator agent used to coordinate and simulate rounds.

Additionally, the directory contains example agents:

* `agent1.txt`, `agent2.txt`, `agent3.txt` → instances of *HonestAgent*.
* `agent4.txt` → instance of *agentRoundSimulator*.

You can add more honest agents by creating files named `agent<i>.txt` (e.g., `agent5.txt`, `agent6.txt`), containing the `"agentHonest"` string.

**Important:** remember to update the initialization code of `agentRoundSimulator` to include one line for each additional honest agent.

**Important:** only the last agent must be the `agentRoundSimulator` one. This is not strictly required, but it sure works :).

---

## Running the Simulation

1. Initialize all agents by triggering the **`initialize`** event to the `agentRoundSimulator`.
2. To simulate one full round (with console output), trigger the following events to `agentRoundSimulator` in order:

   1. `firstbroadcast`
   2. `firstupdatemajority`
   3. `kingbroadcast`
   4. `nextround`

---

## Notes on DALI Events

In DALI, an event is triggered manually using the following syntax:

* **Destination address:** `agent<i>`
* **Sender:** `me` (example)
* **Command:**

  ```bash
  send_message(event, me).
  ```

This allows you to simulate the progression of the algorithm step by step.

