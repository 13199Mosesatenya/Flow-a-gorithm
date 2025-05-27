# Flow-a-gorithm
## Network Traffic Anomaly Detection using Graph-Based Machine Learning

This project presents a robust and extensible system for detecting malicious activities in network traffic. It employs a multi-faceted approach, combining dynamic graph modeling, subgraph isomorphism for pattern matching (both baseline and incremental), and a Graph Convolutional Network (GCN) for advanced classification. The system processes network flow data to identify and analyze suspicious behaviors, providing insights into potential security threats.

### Project Repository Name

`Network-Traffic-Anomaly-Detection`

### Description

This repository provides a comprehensive solution for network anomaly detection, focusing on graph-based techniques. Key components and features include:

* **Data Ingestion and Preprocessing:** [cite: 1, 2, 3]
    * Loads network flow data from a CSV file (e.g., `Wed_ISCX.csv`). [cite: 1]
    * Performs essential data cleaning, including stripping whitespace from column names and handling potential missing values. [cite: 1]
    * Selects crucial features such as `Flow ID`, `Source IP`, `Destination IP`, `Source Port`, `Destination Port`, `Protocol`, `Timestamp`, and `Label`. [cite: 1]
    * Converts the `Timestamp` column to datetime objects and sorts the DataFrame chronologically for time-series analysis. [cite: 1]

* **Dynamic Graph Construction:** [cite: 4]
    * Implements a `create_graph_snapshot` function to transform a window of network flow data into a NetworkX directed graph (`DiGraph`). [cite: 4]
    * Utilizes a `sliding time window` approach to generate a series of graph snapshots, enabling the analysis of evolving network states. [cite: 4] Each node represents an IP address, and edges represent communication flows with attributes like source/destination ports and protocol. [cite: 4]

* **Malicious Subgraph Pattern Definition:** [cite: 5, 6, 7, 8, 9]
    * Defines two distinct malicious subgraph patterns using NetworkX:
        * **Pattern 1 (Internal to Multiple External):** An internal host connecting to multiple external hosts on suspicious (non-standard) ports. This pattern includes a `is_suspicious_port` helper function to identify relevant port ranges. [cite: 5, 7, 8, 9]
        * **Pattern 2 (External to Multiple Internal):** A single external host making multiple connections to internal hosts. [cite: 5, 6, 7, 8, 9]
    * These patterns are designed to represent common attack signatures, such as port scanning or internal reconnaissance.

* **Subgraph Isomorphism-Based Detection:** [cite: 7, 8, 9, 10, 11]
    * **Baseline Detector:** A `check_for_refined_malicious_subgraphs` function performs a full scan on each graph snapshot to detect the predefined malicious patterns using `networkx.algorithms.isomorphism.DiGraphMatcher`. [cite: 7, 8, 9] It includes a stricter `is_suspicious_port_baseline` for comparison purposes. [cite: 7, 8, 9]
    * **Incremental Attack Detector:** An `IncrementalAttackDetector` class provides a more efficient detection mechanism. [cite: 10, 11] It maintains a set of active matches and updates them incrementally based on newly added or removed edges, reducing the computational overhead compared to a full scan. [cite: 10, 11]

* **Simulation and Performance Evaluation:** [cite: 12, 13, 14, 15, 16]
    * A `DynamicGraphModel` simulates the progression of network traffic over time, adding and expiring flows to dynamically update the network graph. [cite: 12, 13, 14, 15, 16]
    * It concurrently runs both the baseline and incremental detection algorithms, recording and comparing their detection times and identified patterns. [cite: 12, 13, 14, 15, 16]
    * The simulation captures ground truth labels for attack snapshots, allowing for comprehensive evaluation of detection accuracy. [cite: 15, 16]

* **Results Visualization and Analysis:** [cite: 17, 18, 19, 20, 21, 22, 23, 24, 25]
    * Includes plotting functionalities to visualize key metrics over time:
        * Network Graph Size (Nodes and Edges). [cite: 18, 19, 20]
        * Detection Latency for both baseline and incremental methods, highlighting the efficiency gains of the incremental approach. [cite: 21, 22]
        * Malicious Pattern Detection status (binary detection for each pattern by each method). [cite: 19, 20]
        * Comparison of Accuracy Metrics (True Positives, False Positives, False Negatives, etc.) for both detection approaches against ground truth. [cite: 23, 24, 25]

* **Graph Neural Network (GCN) for Classification & Gradio Interface:** [cite: 26, 27, 28, 29]
    * **GCNClassifier:** A PyTorch Geometric-based Graph Convolutional Network model designed for graph classification. [cite: 26, 27, 28, 29] It uses `GCNConv` layers, ReLU activation, and dropout for robust feature learning. [cite: 26, 27, 28, 29]
    * **Interactive Prediction Interface:** A Gradio-based web interface allows users to:
        * Upload custom node features and edge list CSV files for graph input.
        * Select to use a default dummy graph for testing.
        * Predict the class (benign/attack) of the input graph using the trained GCN model. [cite: 26]
        * Visualize the input graph. [cite: 26]
        * View predicted class labels and probabilities. [cite: 26]
    * There is also a separate HTML/JavaScript component for an interactive D3.js-based simulation visualization, showing traditional and incremental graphs with real-time metrics. [cite: 30, 31, 32, 33, 34, 35]

### Tag Names

`Python`, `Network Analysis`, `Graph Theory`, `Anomaly Detection`, `Cybersecurity`, `Machine Learning`, `Graph Neural Networks`, `NetworkX`, `PyTorch Geometric`, `Gradio`, `Data Science`, `Dynamic Graphs`, `Subgraph Isomorphism`, `Traffic Monitoring`, `Intrusion Detection System (IDS)`
