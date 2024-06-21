# Dijkstra Algorithm Visualizer

This project is a visual representation of Dijkstra's Algorithm for finding the shortest path between two nodes in a grid. Users can set start and end nodes, add walls, and visualize the algorithm in action.

## Features

- **Set Start Node**: Allows the user to set the starting point for the algorithm.
- **Set End Node**: Allows the user to set the endpoint for the algorithm.
- **Add Walls**: Users can add walls to the grid, making the algorithm find an alternative path.
- **Visualize Dijkstra's Algorithm**: Starts the visualization of Dijkstra's Algorithm from the start node to the end node.

## Structure

- `index.html`: Contains the HTML structure of the page.
- `styles2.css`: Contains the CSS for styling the page.
- `script2.js`: Contains the JavaScript logic for the grid and Dijkstra's Algorithm visualization.

## Installation

1. Clone the repository:
    ```sh
    git clone <repository-url>
    ```

2. Navigate to the project directory:
    ```sh
    cd dijkstra-visualizer
    ```

3. Open `index.html` in your preferred web browser.

## Usage

1. Open the project in your web browser.
2. Click "Set Start Node" and then click on a node in the grid to set the start node.
3. Click "Set End Node" and then click on a node in the grid to set the end node.
4. Click "Add Walls" and then click on nodes in the grid to add walls.
5. Click "Visualize Dijkstra's Algorithm" to see the algorithm in action.

## Code Overview

### HTML

The `index.html` file sets up the structure of the page with a header, a control panel for the buttons, a main section for the grid, and a footer.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dijkstra Algorithm Visualizer</title>
    <link rel="stylesheet" href="styles2.css">
</head>
<body>
    <header>
        <h1>Dijkstra's Algorithm Visualizer</h1>
    </header>
    <div class="controls">
        <button id="startButton">Set Start Node</button>
        <button id="endButton">Set End Node</button>
        <button id="wallButton">Add Walls</button>
        <button id="visualizeButton">Visualize Dijkstra's Algorithm</button>
    </div>
    <main>
        <div id="grid"></div>
    </main>
    <footer>
        <p>© 2024 Dijkstra Algorithm Visualizer. All rights reserved.</p>
    </footer>
    <script src="script2.js"></script>
</body>
</html>
body {
    display: flex;
    flex-direction: column;
    align-items: center;
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: black;
    min-height: 100vh;
    position: relative;
}

header {
    background-color: #282c34;
    color: white;
    padding: 20px;
    text-align: center;
    width: 100%;
}

.controls {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin: 20px 0;
}

button {
    background-color: #fff;
    border: none;
    padding: 10px 20px;
    border-radius: 20px; /* Makes the buttons cylindrical */
    cursor: pointer;
    font-size: 16px;
    transition: background-color 0.3s ease;
}

button:hover {
    background-color: #ddd;
}

main {
    display: flex;
    justify-content: center;
    align-items: center;
    height: calc(100vh - 240px); /* Adjusted to account for the header, buttons, and footer */
    width: 100%;
    flex-grow: 1;
}

#grid {
    display: grid;
    grid-template-columns: repeat(50, 20px);
    grid-template-rows: repeat(20, 20px);
    gap: 2px;
}

.node {
    width: 20px;
    height: 20px;
    border: 1px solid lightgray;
    cursor: pointer;
}

.node.start {
    background-color: green;
}

.node.end {
    background-color: red;
}

.node.wall {
    background-color: gray;
}

.node.visited {
    background-color: purple;
}

.node.shortest-path {
    background: linear-gradient(112.1deg, rgb(32, 38, 57) 11.4%, rgb(63, 76, 119) 70.2%);
}

footer {
    background-color: #282c34;
    color: white;
    padding: 10px;
    text-align: center;
    width: 100%;
    position: absolute;
    bottom: 0;
}
The script2.js file contains the logic for setting nodes, adding walls, and visualizing Dijkstra's Algorithm.
const rows = 20;
const cols = 50;
const grid = [];
let startNode = { row: 0, col: 0 };
let endNode = { row: 19, col: 49 };
let settingStart = false;
let settingEnd = false;
let addingWalls = false;

document.addEventListener("DOMContentLoaded", () => {
    const gridElement = document.getElementById('grid');

    // Initialize grid
    for (let row = 0; row < rows; row++) {
        const currentRow = [];
        for (let col = 0; col < cols; col++) {
            const node = createNode(row, col);
            currentRow.push(node);
            gridElement.appendChild(node.element);
        }
        grid.push(currentRow);
    }

    // Mark start and end nodes
    grid[startNode.row][startNode.col].element.classList.add('start');
    grid[endNode.row][endNode.col].element.classList.add('end');

    document.getElementById('visualizeButton').addEventListener('click', () => {
        resetGrid();
        visualizeDijkstra(grid, startNode, endNode);
    });

    document.getElementById('startButton').addEventListener('click', () => {
        settingStart = true;
        settingEnd = false;
        addingWalls = false;
    });

    document.getElementById('endButton').addEventListener('click', () => {
        settingEnd = true;
        settingStart = false;
        addingWalls = false;
    });

    document.getElementById('wallButton').addEventListener('click', () => {
        addingWalls = true;
        settingStart = false;
        settingEnd = false;
    });

    gridElement.addEventListener('click', handleNodeClick);
});

function createNode(row, col) {
    const nodeElement = document.createElement('div');
    nodeElement.className = 'node';
    nodeElement.dataset.row = row;
    nodeElement.dataset.col = col;
    return {
        row,
        col,
        distance: Infinity,
        isVisited: false,
        isWall: false,
        previousNode: null,
        element: nodeElement
    };
}

function handleNodeClick(event) {
    const element = event.target;
    if (!element.classList.contains('node')) return;

    const row = parseInt(element.dataset.row);
    const col = parseInt(element.dataset.col);

    if (settingStart) {
        grid[startNode.row][startNode.col].element.classList.remove('start');
        startNode = { row, col };
        element.classList.add('start');
        settingStart = false;
    } else if (settingEnd) {
        grid[endNode.row][endNode.col].element.classList.remove('end');
        endNode = { row, col };
        element.classList.add('end');
        settingEnd = false;
    } else if (addingWalls) {
        if (!element.classList.contains('start') && !element.classList.contains('end')) {
            element.classList.toggle('wall');
            grid[row][col].isWall = !grid[row][col].isWall;
        }
    }
}

function resetGrid() {
    for (let row = 0; row < rows; row++) {
        for (let col = 0; col < cols; col++) {
            const node = grid[row][col];
            node.distance = Infinity;
            node.isVisited = false;
            node.previousNode = null;
            node.element.classList.remove('visited', 'shortest-path');
        }
    }
}

function visualizeDijkstra(grid, startNode, endNode) {
    const unvisitedNodes = [];
    grid[startNode.row][startNode.col].distance = 0;

    for (const row of grid) {
        for (const node of row) {
            if (!node.isWall) {
                unvisitedNodes.push(node);
            }
        }
    }

    animateDijkstra(unvisitedNodes, grid, endNode);
}

function animateDijkstra(unvisitedNodes, grid, endNode) {
    if (unvisitedNodes.length === 0) return;

    unvisitedNodes.sort((a, b) => a.distance - b.distance);
    const
