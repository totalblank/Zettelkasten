GNNs are a type of neural network designed to perform machine learning tasks on graph data structures. They are particularly useful for tasks where data is represented as graphs, such as social networks, molecular structures, and recommendations systems.

The key idea behind GNNs is to capture the dependencies between the connections in the graph. They  do this by **aggregating the features of neighboring nodes to generate embeddings for each node**. These embeddings can then be used to perform various tasks such as node classification, link prediction, and graph classification.

Step-by-step process of how GNNs work:

1. **Node Feature Initialization:** Each node in the graph is initialized with a feature vector. This could be a one-hot encoding of the node's label, some real-valued vector specified to the node, or even a vector of zeros.
2. **Feature Aggregations:** Each node aggregates the feature vectors of its neighboring nodes to update its own feature vector. This is typically done using a function that takes in the features vectors of the node and its neighbors and outputs a new feature vector. The function could be a simple average, a weighted sum, or a more complex function.