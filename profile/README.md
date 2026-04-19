# **Overview**

- Different from traditional education strategies, this project emphasizes the
  importance of motivation-driven learning and gaining hands-on experience and
  advanced motivation through building projects. The learning path is designed
  as a tree structure, where each node represents a specific topic or skill.
  Users can choose their own learning path based on their interests and goals,
  and the system will provide personalized guidance and resources to help them
  achieve their objectives based on a top-to-down approach.

# **Philosophy**

- The core philosophy of this project is to encourage users to get rid of the
  current corrupted and inefficient education system and form a personalized
  comfort zone for learning and building. And finally achieve the real
  democratization of education and opportunities.
- ### What we are against:
  1. The overestimated significance of study
  2. The one-size-fits-all and extremely rigid and corrupted education system
  3. Education systems that are designed for the convenience of teachers and
     education bureaus rather than students
  4. The lack of hands-on experience and motivation-driven learning
  5. Some popular and widely accepted perspectives about learning and education
- ### What we are for:
  1. The underestimated significance of building and hands-on experience
  2. The personalized and flexible learning paths based on users' interests and
     goals
  3. Education systems that are designed for the convenience of students rather
     than teachers and education bureaus
  4. The importance of motivation-driven learning and it should be way more
     crucial than study
  5. Some unpopular but more accurate perspectives about learning and education
  6. Knowledge transition between people costs.
  7. The platform does not teach.
  8. It provides plans and routes for building.
  9. Anti-Bureau: the system optimizes for legibility, not ceremony.
  10. Motivation-driven: the system starts from intent, not content inventory.
  11. Hard blocks encountered during building are recursively materialized as
      child nodes.

# **Learning Tree**

- Root node: initial user interest.
- Generation strategy: BFS. Only the next level matters.
- Child nodes must fully represent all prerequisites of the current node.
- If a parent is too hard to take as a single unit, materialize children.
- If a parent is ok to take, do not generate children.
- Children divide hard nodes into easier independent nodes.
- The tree is the core product engine.

# **Node Contract**

| Field                     | Description                                                                            |
| ------------------------- | -------------------------------------------------------------------------------------- |
| Topic                     | The current unit of intent                                                             |
| Property                  | Mandatory or Optional                                                                  |
| Signs of Completion (SoC) | The hard boundary that defines when a node is realized                                 |
| Related Nodes             | Vague horizontal relations at similar tree depths, activated when user interest shifts |

# **Property semantics:**

- Mandatory: unskippable state, though it may be replaced by an equivalent
  alternative.
- Optional: not required individually, but a quota of optional nodes at a level
  can aggregate into a mandatory requirement.

# **Traversal triggers:**

- processed → step_up
- disappointed → step_down
- branch mismatch → go_around

# **Interaction**

The studio is a game loop: observe, choose, execute, verify, update.

Canonical flow: Intent Handshakes → Step_down → User Loop → Done

- step_down materializes when the current node cannot be accepted as a single
  unit.
- User Loop is the return path where the user continues decomposition or accepts
  the current node.
- Done only exists when the node returns a valid SoC.

# **Architecture**

- **Level 0**: Evaluation Engine: `Agent_Trace` → `Normalized_State`

- **Level 1**: Streaming Protocol: `Normalized_State` → `Delta_Event_Stream`

- **Level 2**: Studio Orchestrator: `Delta_Event` → `ReactFlow`, `R3F`, `GSAP`

- ### The layers stay pure by separating reduction, transport, and rendering.

## 2. Most Common Use Case

```mermaid
sequenceDiagram
    autonumber
    participant c1 as User
    participant c2 as Web
    participant c3 as Sandbox
    participant c4 as External Agents
    participant c5 as Background Agents
    participant c6 as Database
    participant c7 as Call Step With Arguments

    Note over c1, c6: Step 1 (Intend Analysis / Handshakes)
    c1->>c4: Sends interest (eg. learning or building something)
    loop Continuous Analysis & Retrieval & Requests (Break when external agents confirm that local context is sufficient)
        c4->>c6: Requests high level retrieval
        activate c6
        c6-->>c4: Returns high level data
        deactivate c6
        par requesting further details
            c4->>c2: Sends enquiries
            activate c2
            c2->>c1: Sends formatted enquiries
            deactivate c2
        and managing local context
            c4->>c4: Saves organized updated context to local storage
        end
        c1->>c4: Sends follow-up info
        c4->>c4: Saves improved context to local storage
        opt If external agents think local context is sufficient
            c4->>c7: Jump to step 2 with step down command
        end
    end

    Note over c1, c6: async iteration: Step 2 (Send)
    loop Continuous decisions drafting (yields when a node is ready to be sent to frontend studio, break when external agents confirm the completion of nodes generation)
        c4->>c5: Sends command given by arguments (default: step_down) & essential info
        activate c5
        loop generate required nodes for current node based on given command (eg. generate sub-nodes when receiving step_down) break when all required nodes are generated
            par generating nodes
                c5->>c6: Requests low-level retrieval
                activate c6
                c6-->>c5: Returns low-level data
                deactivate c6
                c5->>c5: Internal computing
                c5->>c4: Sends computing results & feedbacks
            and managing local context
                c4->>c4: Saves updated context to local storage
            end
        end
        deactivate c5
        c4->>c3: Calls sandbox to initialize frontend studio for users by generating key codes & necessary info
        c3->>c2: Sends built dist
        c2->>c1: Set up frontend studio for users
        loop Hooks: Continuous Monitoring
            c5->>c4: Monitors local storage
            c5->>c6: Sends updated data (if diffs & agreed)
        end
    end

    Note over c1, c6: Step 3 (Receive)
    loop Continuous interaction between users and frontend studio (Break when users complete the task or need to jump to other steps)
        par Client side interactions
            loop User Interactions (Break when external agents need to pause and interact with users)
            par Sending feedbacks in texts
            c1->>c4: Sends feedbacks
            and Interacting with frontend studio
            c1->>c3: Interacts with sandbox
            c3->>c4: Sends interaction results
            c4->>c4: Evaluates feedbacks & results (local storage)
            end
            end
        and Server side interactions

        alt Disappointed by evaluation (Current node is overwhelming for users and needs to break down into sub-nodes)
            c4-->>c7: Jump to Step 2 with step_down command (Context/States unchanged)
        else Not disappointed
            c4->>c4: Saves updated context to local storage
        end

        loop Hooks: Continuous Monitoring & Sync
            c5->>c4: Monitors local storage
            activate c5
            c5->>c6: Request low-level retrieval (on update)
            c6-->>c5: Returns data
            c5->>c6: Sends updated data (if diffs & agreed)
            c5->>c4: Sends useful context
            deactivate c5
            c4->>c4: Saves updated context to local storage
            c4-->>c3: Sends wanted modifications of frontend studio when needed
        end
        end

        alt If current node (not root node) is completed
            c4->>c7: Sends step_up & go_around (overriding Step 2)
        else If current node (root node) is completed
            c4-->>c7: Jump to step 4 since there's no incomplete node
        end
    end

    Note over c1, c6: Step 4 (Done)
    c5->>c4: Gets local storage
    c5->>c6: Sends updated data
    c4->>c2: Sends done signal
```

## 3. Best Practices Of Nodes Making Up A Tree

```mermaid
mindmap
  root((Initial User Interest))
    AI Systems
      Topic: AI Systems
      SoC: a concrete build route is selected
      Prerequisites
        Linear Algebra
          Topic: Linear Algebra
          SoC: vector and matrix operations are usable in a build
          Vectors and Matrices
            Topic: Vectors and Matrices
						SoC: dot product, matrix multiply, and shape checks pass
          Eigen-decomposition
            Topic: Eigen-decomposition
            SoC: eigen basis supports PCA or compression
            go_around
              Trigger: alternate decomposition required
        Databases
          Topic: Databases
          SoC: schema and queries are verified
          Relational
            Topic: Relational
            SoC: connection pooler and query plan are validated
            Query Optimization
              Topic: Query Optimization
              SoC: N+1 is removed and the explain plan is clean
          Non-Relational
            Topic: Non-Relational
            SoC: document shape and index strategy are defined
        DevOps
          Topic: DevOps
          SoC: build, test, and release are reproducible
          Cloud Infrastructure
            Topic: Cloud Infrastructure
            SoC: environments, secrets, and observability are provisioned
      Traversal Control
        step_down
          Trigger: parent not solved as a single unit
        step_up
          Trigger: SoC processed
        go_around
          Trigger: alternate decomposition needed
```
