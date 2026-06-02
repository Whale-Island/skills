# Game Development Problem Patterns

A reference library of common problem patterns in game development. Use this to accelerate Phase 1 decomposition by matching observed symptoms to known patterns.

## Rendering Domain

### Pattern: Draw Order / Z-Fighting
- **Symptoms**: Flickering surfaces, objects appearing behind/in front incorrectly
- **Common Root Causes**: Z-buffer precision, render priority misconfiguration, overlapping positions
- **Decomposition Hint**: Check material render priority, camera near/far planes, spatial overlap

### Pattern: Shader Compilation Errors
- **Symptoms**: Pink/magenta materials, console shader errors, missing textures
- **Common Root Causes**: Shader syntax errors, missing uniforms, platform incompatibility
- **Decomposition Hint**: Check shader source, uniform bindings, target platform

### Pattern: Memory Leaks in Visuals
- **Symptoms**: Increasing VRAM usage, eventual crash, degraded frame rate over time
- **Common Root Causes**: Unreleased textures, unbounded particle pools, missing Dispose calls
- **Decomposition Hint**: Profile VRAM, check resource lifecycle, verify cleanup in OnDestroyed

## Physics Domain

### Pattern: Tunneling
- **Symptoms**: Fast objects passing through colliders, missed collisions
- **Common Root Causes**: Discrete collision detection at high velocities, thin colliders
- **Decomposition Hint**: Check physics step size, collider thickness, continuous detection mode

### Pattern: Jitter / Instability
- **Symptoms**: Objects vibrating, inconsistent physics behavior
- **Common Root Causes**: Competing forces, precision errors, multiple physics steps per frame
- **Decomposition Hint**: Check force application order, floating point precision, physics substep count

### Pattern: Collision Detection Misses
- **Symptoms**: Objects not colliding when they should, signals not firing
- **Common Root Causes**: Layer mask misconfiguration, missing collision layers, signal not connected
- **Decomposition Hint**: Verify collision layers/masks, check signal connections, test with debug shapes

## Gameplay Domain

### Pattern: State Desync
- **Symptoms**: Client and server disagree on game state, actions not registering
- **Common Root Causes**: Race conditions, missing state synchronization, event ordering
- **Decomposition Hint**: Trace state transitions, check event order, verify sync points

### Pattern: Rule Violations
- **Symptoms**: Players can perform actions they shouldn't, constraints not enforced
- **Common Root Causes**: Missing validation, bypassed checks, incorrect condition logic
- **Decomposition Hint**: Trace the action path, find where validation should occur, check all entry points

### Pattern: Infinite Loops / Deadlocks
- **Symptoms**: Game freezes, infinite recursion, stack overflow
- **Common Root Causes**: Circular dependencies, missing termination conditions, mutual waits
- **Decomposition Hint**: Check loop conditions, dependency graphs, recursive base cases

## UI Domain

### Pattern: Layout Breaks on Resize
- **Symptoms**: Elements overlapping, text cut off, misaligned at different resolutions
- **Common Root Causes**: Hard-coded sizes, missing anchors, incorrect container sizing
- **Decomposition Hint**: Check anchor modes, container sizing strategy, minimum size settings

### Pattern: Stale Display
- **Symptoms**: UI showing outdated data, values not updating after changes
- **Common Root Causes**: Missing signal connections, data binding not refreshed, cached values
- **Decomposition Hint**: Check signal connections, verify data flow, look for cached/stale references

### Pattern: Input Not Registering
- **Symptoms**: Button clicks ignored, keyboard input not captured, touch events missed
- **Common Root Causes**: Input consumed by parent, wrong focus, event propagation stopped
- **Decomposition Hint**: Check input handling order, focus state, mouse filter settings

## Data / Config Domain

### Pattern: Cross-Reference Errors
- **Symptoms**: Missing entities, null references, "not found" errors
- **Common Root Causes**: ID mismatches, missing entries, deleted but still referenced
- **Decomposition Hint**: Verify all foreign key references, check for orphaned entries, validate consistency

### Pattern: Type Mismatches
- **Symptoms**: Cast errors, wrong values, unexpected behavior
- **Common Root Causes**: CSV parsing errors, schema drift, implicit conversions
- **Decomposition Hint**: Compare schema vs actual data, check parser logic, verify type expectations

### Pattern: Loading Order Dependencies
- **Symptoms**: Works sometimes but not always, depends on scene load order
- **Common Root Causes**: Initialization order, missing ready checks, race conditions in loading
- **Decomposition Hint**: Check initialization sequence, verify Ready/EnterTree order, add guards

## Performance Domain

### Pattern: Frame Rate Drops
- **Symptoms**: FPS drops, stuttering, inconsistent frame times
- **Common Root Causes**: Garbage collection spikes, excessive allocations, blocking operations on main thread
- **Decomposition Hint**: Profile frame times, check GC allocation, identify hot paths

### Pattern: Memory Growth
- **Symptoms**: Increasing RAM usage over time, eventual OOM crash
- **Common Root Causes**: Unreleased references, growing collections, missing cleanup
- **Decomposition Hint**: Take memory snapshots, compare over time, check collection growth

### Pattern: Loading Stalls
- **Symptoms**: Long load times, frozen screen during transitions
- **Common Root Causes**: Synchronous loading, large resource loading, blocking I/O
- **Decomposition Hint**: Check load operations, verify async patterns, profile load times

## Lifecycle Domain

### Pattern: Null Reference on Access
- **Symptoms**: NullReferenceException, "object disposed" errors
- **Common Root Causes**: Accessing freed nodes, timing issues, missing null checks
- **Decomposition Hint**: Check node lifecycle, verify IsInstanceValid, add null guards

### Pattern: Double Initialization
- **Symptoms**: State reset unexpectedly, events firing twice, duplicate entries
- **Common Root Causes**: Ready called multiple times, scene reinstantiation, signal connected twice
- **Decomposition Hint**: Check Ready logic, verify signal connection mode, add init guards

### Pattern: Cleanup Failures
- **Symptoms**: Ghost objects, lingering effects, resource leaks
- **Common Root Causes**: Missing OnDestroyed/Dispose, unsubscribed events, orphaned timers
- **Decomposition Hint**: Check cleanup paths, verify event unsubscription, trace resource lifecycle

## Cross-Domain Patterns

### Pattern: Configuration Drift
- **Symptoms**: Behavior doesn't match documentation, unexpected defaults
- **Common Root Causes**: Config not reloaded after change, hardcoded defaults, multiple config sources
- **Decomposition Hint**: Verify config loading, check for hardcoded overrides, validate config precedence

### Pattern: Serialization Loss
- **Symptoms**: Save/load data missing, properties reset after deserialization
- **Common Root Causes**: Missing [Export] attributes, non-serializable types, version mismatch
- **Decomposition Hint**: Check serialization attributes, verify save format, test round-trip

### Pattern: Multi-System Interaction Bugs
- **Symptoms**: Bug only occurs when multiple systems are active simultaneously
- **Common Root Causes**: Shared state mutation, event ordering, resource contention
- **Decomposition Hint**: Identify all systems involved, trace shared state, check interaction points

## Map/Layout Domain

### Pattern: Post-Hoc Data Trimming Breaks Connectivity
- **Symptoms**: Connections disappear after "cleanup" step, nodes become isolated, graph fragments
- **Common Root Causes**: Applying max-connection limits or direction filters AFTER the adjacency graph is fully built, without understanding which connections are structurally essential
- **Decomposition Hint**: NEVER trim adjacency data after generation. Instead, control the generation process itself to produce the desired structure. If you must limit connections, do it at the point of ADDITION, not after the fact
- **Key Rule**: Data integrity must be preserved throughout the pipeline. Post-hoc trimming is a symptom of insufficient generation control, not a valid fix

### Pattern: Search Origin Offset Produces Diagonal Positions
- **Symptoms**: Grid layout shows 8-directional (diagonal) positions despite using 4-directional candidate generation
- **Common Root Causes**: Candidate search uses placed neighbors as search origins instead of the hub core. GenerateCardinalRing(neighborPos, ring, step) produces positions that are cardinal relative to the neighbor but diagonal relative to the core
- **Decomposition Hint**: When generating grid positions, always search from the reference point (core/center), not from already-placed neighbors. Use neighbor proximity for SCORING, not for SEARCH ORIGIN

### Pattern: Cascade Push Over-Spreading
- **Symptoms**: Hubs pushed far from center, map becomes sparse, distances between hubs much larger than necessary
- **Common Root Causes**: When pushing hub B away from hub A, also pushing ALL hubs "beyond" B in the push direction. This over-corrects and creates excessive spacing
- **Decomposition Hint**: Only push the directly conflicting hub. Use iterative resolution (multiple passes) instead of cascade push. Each iteration resolves one conflict; convergence is guaranteed because total spread increases monotonically

### Pattern: Non-Deterministic Random Breaks Seed Consistency
- **Symptoms**: Same seed produces different layouts across runs, cannot reproduce bugs
- **Common Root Causes**: Using Guid.NewGuid(), new Random() (no seed), GD.Randi(), or DateTime-based random instead of seeded System.Random
- **Decomposition Hint**: Audit ALL random sources. Replace non-deterministic sources with seeded rng. Pass rng through all methods that need randomness. Sort Dictionary/HashSet iterations for deterministic order

### Pattern: Dictionary/HashSet Iteration Order Non-Determinism
- **Symptoms**: Layout differs between process runs with same seed, but consistent within a single run
- **Common Root Causes**: .NET Dictionary and HashSet iteration order depends on internal hash bucket layout, which varies between process runs due to hash randomization
- **Decomposition Hint**: Sort keys before iterating when iteration order affects decisions. Use OrderBy() on keys/values before processing. This is especially critical in layout algorithms where position assignment depends on processing order

### Pattern: BFS Reachability Must Traverse Intermediate Nodes
- **Symptoms**: Reachability check reports nodes as unreachable even though they are connected through corridor/bridge nodes
- **Common Root Causes**: BFS only iterates over primary region dict, excluding corridor/bridge/portal intermediate nodes from traversal. The BFS cannot cross these nodes to reach regions on the other side
- **Decomposition Hint**: Build a COMPLETE adjacency graph that includes ALL node types (regions + corridors + portals) before BFS. If the graph has intermediate node types, they MUST be included in traversal. Test: pick the farthest node from start and verify BFS reaches it
- **Key Rule**: Any node type that appears in adjacency chains must be traversable in BFS, or reachability detection is invalid

### Pattern: Node Repositioning Breaks Adjacency Chains
- **Symptoms**: After compaction/repositioning, some nodes become isolated pairs or lose connections to their hub core
- **Common Root Causes**: Algorithms like RecompactHubs or ResolveOverlaps move nodes to new positions but only check distance-to-core, not distance-to-neighbors. A node moved from 2-step position to 1-step position may lose its 1-step adjacency to the node that was at 1-step
- **Decomposition Hint**: After ANY position change, verify that the moved node still has at least one 1-step cardinal neighbor. Use BFS-style placement: place each node adjacent to an already-placed node, ensuring continuous adjacency chains from core to all members
- **Key Rule**: Position changes must preserve 1-step adjacency chains. Moving a node without checking neighbor distances is a connectivity violation

### Pattern: Portal/Teleporter Position Overlap
- **Symptoms**: ADJ-ERROR or DIAGONAL-WARN between portal and exit node, dx=0 dy=0 (same position)
- **Common Root Causes**: Portal nodes placed at the exact same position as the exit node they connect to, instead of offset by 1 grid step. The portal should be adjacent to the exit, not overlapping it
- **Decomposition Hint**: When creating portal/teleporter pairs, always offset the portal position by 1 GridStep from the exit node in the direction toward the other hub. If the offset position is occupied, try other cardinal directions
- **Key Rule**: Portal position = exit position + 1 GridStep offset. Two nodes at the same position is always an error

### Pattern: Warn-Only Validation Allows Invalid State
- **Symptoms**: Invalid connections (diagonal, non-cardinal) appear in the visual output despite being detected and logged as warnings
- **Common Root Causes**: Validation code detects invalid connections but only logs warnings without preventing them from being used. The invalid data flows into rendering/caching systems and produces incorrect visual output
- **Decomposition Hint**: When a connection violates structural constraints (direction, distance), it must be BLOCKED from entering the data pipeline, not just warned about. Use `continue`/`break` to skip invalid entries, not just log them
- **Key Rule**: Detection without prevention is useless. If a connection is invalid, it must be excluded from the data, not just flagged

### Pattern: Unbounded Layout Expansion
- **Symptoms**: Nodes placed at extreme coordinates (5000+, -5000+), map becomes sparse, corridors become excessively long
- **Common Root Causes**: No global boundary constraint on node positions. Push/separation algorithms can push nodes arbitrarily far. Overlap resolution can shift entire clusters to distant positions
- **Decomposition Hint**: Define a MaxMapRadius constant and clamp all position changes within it. Add boundary checks in EnforceHubSeparation, ResolveOverlaps, and any position mutation. Validate that all nodes are within bounds at the end
- **Key Rule**: Every position mutation must respect global bounds. Without boundaries, layout algorithms have no convergence guarantee

### Pattern: Physical Distance Calibration Requires Multi-Layer Guarantees
- **Symptoms**: Adjacent nodes have distances that are not exact multiples of GridStep, causing AP calculation errors
- **Common Root Causes**: Position mutations (push, overlap resolution, compaction) produce non-GridStep-aligned coordinates. Single-layer alignment (e.g., only at the end) misses intermediate mutations
- **Decomposition Hint**: Apply GridStep alignment at THREE points: (1) after each position mutation (push/resolve), (2) as a global pass after all mutations, (3) as a validation check at the end. Each layer catches what the previous missed
- **Key Rule**: 1 AP = 1 GridStep is a HARD constraint. Multi-layer alignment ensures this invariant holds throughout the pipeline
