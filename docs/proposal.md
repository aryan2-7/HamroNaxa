# Project Proposal, Rough Draft

**Title:** HamroNaxa

**Course:** ENGG 102
**Team:** Aryan, Khaniya, DK, Nasala, Shresh (add roll numbers)
**Submitted to:** (coordinator's name)
**Date:** (add)

---

## Abstract (aim for 150-200 words, this is ~170)

*Background:* Navigation apps like Google Maps use shortest-path algorithms, but users only ever see the final route, never how the algorithm found it. For students learning graph algorithms, this is a black box. *Purpose:* We propose a web app that models the Kathmandu University campus(maybe further too) as a graph and lets users pick two locations and an algorithm (Dijkstra, A*, bidirectional search), then watches the search animate step by step. *Method:* Campus locations and paths are collected by the team and stored as a weighted graph. The backend runs the algorithms and records each step, and the frontend plays these steps as an animation, drawing the final shortest path in a different color. A smart search feature helps users find locations using partial or misspelled names. *Expected outcome:* A working, deployable web app with a real KU campus map, several algorithms that can be compared side by side, and clear animations. *Conclusion:* The project combines data structures, algorithms and web development into a useful and educational tool. *Recommendation:* It can later be extended to indoor maps or other campuses.

**Keywords:** shortest path, Dijkstra, A*, graph visualization, campus navigation

---

## 1 Introduction

### 1.1 Background (needs 200-400 words later, just ideas for now)

- Navigation = shortest path problem on a graph (nodes = places/junctions, edges = roads/paths, weights = distance or time)
- Classic algorithms: Dijkstra (1959), A* (1968, adds a heuristic), bidirectional search (searches from both ends)
- Real-world: Google Maps, OpenStreetMap-based routers, game pathfinding
- Trend: interactive visualizers for learning algorithms (VisuAlgo, pathfinding visualizers on GitHub) are popular
- Drawbacks of existing stuff:
  - Google Maps doesn't show how a route is found
  - Generic visualizers use blank grids, not real maps
  - Neither covers KU campus, and Google Maps often doesn't have footpaths and shortcuts inside campus
- Our gap: a real campus map + algorithm animation + comparison in one place

### 1.2 Objectives

1. Model the KU campus as a weighted graph using real collected data
2. Implement multiple shortest-path algorithms (Dijkstra, A*, bidirectional) and let users choose
3. Visualize the search step by step and show the final path in a different color
4. Provide smart location search and a simple, deployable web interface
5. (Not sure) Include a simple ai in the search tab where we can explain what we want to go in simple language and it will route accordingly

### 1.3 Motivation and Significance

- Why this topic: we're all studying DSA right now and graph algorithms feel abstract until you see them run
- Helps new students find their way around campus (blocks, labs, canteen, hostel, etc.)
- Different from existing work: real local map, side-by-side algorithm comparison (nodes visited, path length, time), built by us from scratch
idk add some more maybe if needed

### 1.4 Expected Outcomes

- Working web app (hopefully deployed online)
- Real KU campus graph dataset (reusable by others)
- Animated visualization of the working of the algorithms
- Comparison stats (nodes explored, path length, run time)
- Documentation + presentation

---

## 2 Related Works

*(Use APA in the final version. These are real references, but double-check the details before citing.)*

- **Dijkstra (1959):** original shortest-path algorithm, "A note on two problems in connexion with graphs." Guarantees the shortest path but explores in all directions.
- **Hart, Nilsson & Raphael (1968):** A* search, uses a heuristic (e.g., straight-line distance) to explore fewer nodes.
- **Pohl (1971):** bidirectional search, runs from both start and goal and meets in the middle.
- **Google Maps / OpenStreetMap:** real-world routing, but the algorithm is hidden and campus footpaths are poorly covered.
- **VisuAlgo / pathfinding visualizers:** good for teaching, but mostly use grids, not real maps.
- **Comparison idea:** small table of Google Maps vs VisuAlgo vs ours (real map? shows algorithm? campus-specific? compare algorithms?)

---

## 3 Procedure and Methods

1. **Collect data:** first use open street maps, and if we have time and motivation we plot the locations, list important places (nodes), note paths between them (edges), estimate distances (GPS coordinates or Google Maps measurements). Shared by all members.
2. **Build the graph:** adjacency list; start with random test nodes/edges, then swap in real KU data.
3. **Algorithms:** implement Dijkstra, A* (Euclidean/haversine heuristic), bidirectional. Each one logs its steps (visited node, frontier, current path) so the UI can replay them.
4. **Smart search:** the AI/ML part, tentative options:
   - fuzzy matching on location names (typos, nicknames like "canteen")
   - or simple text-similarity / small ML model to match queries to places
   - stretch: suggest the best algorithm or predict a route time
5. **Backend + integration:** API that takes (start, end, algorithm) and returns the path + step log.
6. **Frontend:** map view, node/edge drawing, dropdowns, speed slider, play/pause/step, final path highlighted in a different color.
7. **Testing:** compare results across algorithms (all should give the same shortest distance), test edge cases (no path, same start and end).
8. **Deploy + document.**


<img width="2055" height="1502" alt="system-architecture" src="https://github.com/user-attachments/assets/9500f813-59a0-4cba-a913-7e20fb95986c" />
<img width="1434" height="2196" alt="flowchart-user-flow" src="https://github.com/user-attachments/assets/2531ada1-d4eb-4a1f-99d9-d590b7fd4d1a" />


---

## 4 System Requirement Specifications

### 4.0.1 Software Specifications

**Functional:**
- Select start and end locations (dropdown, click on map, or search)
- Select the algorithm
- Animate the search + show final path in a distinct color
- Show stats: distance, nodes visited, time taken
- Adjustable animation speed, pause/step
- (Maybe) compare two algorithms side by side
- (Maybe) admin/dev mode to add nodes and edges

**Non-functional:**
- Fast response, smooth animations
- Works in modern browsers, also usable on mobile
- Easy to deploy (GitHub Pages / Vercel / similar)
- Code is modular so several people can work at once

**Dependencies (tentative, decide as a team):** frontend framework + canvas/SVG or a map library, backend language, Git/GitHub, JSON data files.

**Use-case diagram to add:** 
<img width="2287" height="1426" alt="use-case-diagram" src="https://github.com/user-attachments/assets/8b6d3998-c730-4bb6-9588-267c7ea33431" />


`<<include>>` means the main use case always needs the other one as a required step. The arrow goes from the main use case to the included one. Running pathfinding always needs a start and end, an algorithm, the animation and the final result, so it now points to all four.

`<<extend>>` means an optional add-on that only happens sometimes. The arrow goes from the optional use case to the one it extends. Search location is an optional way to select locations, Compare algorithms is optional after a run, and Adjust speed is optional during the animation.

### 4.0.1.5 Tech Stack hahadost
Python (3.11+)
The primary programming language for all core logic: the graph data structure, the pathfinding algorithms (Dijkstra, A*, bidirectional search), the step recorder that logs every visited node and relaxed edge for the animation, the smart search engine, and data loading. Each algorithm lives in its own plain Python module with no web code inside, so team members can develop and test them independently.

Flask
A lightweight Python web framework that serves the web app and exposes the routes the browser calls: fetching the campus graph, running a chosen algorithm between two locations and returning its steps as JSON, and handling search queries. Flask is simple enough for the whole team to learn quickly.

HTML, CSS and JavaScript (Canvas)
The browser front end. HTML and CSS build the interface: location pickers, algorithm selector, speed slider, and play/pause/step controls. A small JavaScript canvas script replays the step list returned by the backend, coloring visited nodes, the frontier, and the final shortest path in a distinct color. All pathfinding logic stays in Python, and JavaScript only handles drawing and playback.

Python Standard Library
Core data structures and utilities: dict for the adjacency list, heapq for the priority queue used by Dijkstra and A*, collections.deque for queue-based traversal, set for visited-node tracking, and json for reading and writing graph data files.

JSON Data Files
The campus map is stored as JSON: nodes with names, coordinates, and aliases, and edges with distances. The same format holds the random test graphs used early on and the real Kathmandu University campus data collected later, so swapping one for the other needs no code changes.

pytest
Automated testing for the algorithms. Tests check that Dijkstra, A*, and bidirectional search all return the same shortest distance on the same graphs, and that edge cases such as unreachable nodes and identical start and end are handled correctly.

Git / GitHub
Version control for tracking changes across all components. Each team member works on a dedicated feature branch with pull-request-based integration into main, and tasks are tracked as GitHub Issues. Hosting and deployment will be decided after the main parts of the project are complete.

### 4.0.2 Hardware Specifications

- Software-only project, no special hardware
- Any laptop that can run a browser and a code editor
- Phone/laptop with GPS or a measuring app for data collection

---

## 5 Project Planning and Scheduling

**Gantt chart (14 weeks), make it as a figure later:**

| Task | W1-2 | W3-4 | W5-6 | W7-8 | W9-10 | W11-12 | W13-14 |
|---|---|---|---|---|---|---|---|
| Proposal + research | X | | | | | | |
| Campus data collection | X | X | X | | | | |
| Graph + algorithms | | X | X | X | | | |
| UI / visualization | | | X | X | X | | |
| Smart search | | | | X | X | | |
| Backend + integration | | | | | X | X | |
| Testing + debugging | | | | | | X | X |
| Documentation + presentation | | | | | | X | X |

### 5.1 Tasks

1. Problem identification and requirement gathering
2. Problem analysis (what graph, what algorithms, what the UI needs)
3. Design (graph format, API, UI mockups)
4. Data collection (shared)
5. Implementation (algorithms, UI, smart search, backend)
6. Integration (git branches, merge into main)
7. Testing and debugging
8. Documentation and final presentation

*(Task pool idea: each member picks 2-3 tasks, UI/UX and data collection can be shared.)*

---

## References (APA, verify before using)

- Dijkstra, E. W. (1959). A note on two problems in connexion with graphs. *Numerische Mathematik, 1*(1), 269-271.
- Hart, P. E., Nilsson, N. J., & Raphael, B. (1968). A formal basis for the heuristic determination of minimum cost paths. *IEEE Transactions on Systems Science and Cybernetics, 4*(2), 100-107.
- Pohl, I. (1971). Bi-directional search. *Machine Intelligence, 6*, 127-140.
