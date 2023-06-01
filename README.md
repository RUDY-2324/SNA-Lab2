# SNA-Lab2


## LAB - 2
## 1.Newman-Girvan algorithm


The Newman-Girvan algorithm, also known as the Girvan-Newman algorithm or the edge betweenness algorithm, is a widely used algorithm for community detection in networks. It was developed by Mark Newman and Michelle Girvan in 2002.
The algorithm is based on the concept of "edge betweenness centrality," which measures the importance of edges in connecting different communities. The basic idea is that edges with high betweenness centrality are more likely to lie between communities and therefore can be considered as potential boundaries between communities.
Here are the steps of the Newman-Girvan algorithm:

1.Calculate the betweenness centrality for all edges in the network. This involves finding the shortest paths between all pairs of nodes and determining how many of those paths pass through each edge.

2.Remove the edge with the highest betweenness centrality from the network. This effectively disconnects the network into two or more components.

3.Calculate the betweenness centrality for all remaining edges in the updated network.

4.Repeat steps 2 and 3 until all edges have been removed or until a desired number of communities have been identified.

The resulting disconnected components are considered as communities or subnetworks.

## 2.Louvain algorithm

The Louvain algorithm is a popular community detection algorithm that aims to find the modular structure of a network. It is an iterative algorithm that optimizes the modularity of the network by iteratively merging and reassigning nodes to communities. Here are the general steps of the Louvain algorithm:

Initialization: Start with each node in the network assigned to its own community.

Modularity calculation: Calculate the modularity of the network in its current state. Modularity measures the quality of a division of a network into communities. It compares the number of edges within communities to the expected number of edges in a random network.

Iteration:

For each node, calculate the modularity gain of removing it from its current community and placing it in the neighboring communities. The modularity gain is the improvement in modularity that would result from moving the node.
Move the node to the community that gives the maximum modularity gain. If the modularity gain is negative or zero for all neighboring communities, leave the node in its current community.
Repeat the above steps for all nodes in the network.
Community aggregation: After completing one pass over all nodes, each community is considered as a single node in a new network. The weights of the edges between communities are the sum of the weights of the original edges between the nodes in the communities.

Repeat steps 2 to 4: Iterate the process of modularity calculation, node reassignment, and community aggregation until no further improvement in modularity is observed. This means that the algorithm has converged and found the optimal community structure.

Post-processing: Once the Louvain algorithm has converged, the final community structure is obtained. Nodes within the same community are grouped together.

It's worth noting that the Louvain algorithm is a heuristic method and may not always find the global optimum, but it often produces good results in practice


## 3.LIBRARIES USED AND DESCRIPTION:
a. networkX : NetworkX is a package for the Python programming language that's used to create, manipulate, and study the structure, dynamics, and functions of complex graph networks.
b. Count Vectorizer is used to convert documents, text into vectors of term or token counts, it involves counting the number of occurences of words appears in a document
c. Latent Dirichlet Allocation (LDA): LDA is a generative probabilistic model that assumes each topic is a mixture over an underlying set of words, and each document is a mixture of over a set of topic probabilities.



## 4.Newman-Girvan algorithm PSUEDOCODE:
Input: Social network graph G

function edge_betweenness_centrality(G):
    betweenness_scores = {}  // Dictionary to store edge betweenness scores
    
    for edge in G.edges:
        betweenness_scores[edge] = calculate_betweenness(edge, G)
    
    return betweenness_scores

function calculate_betweenness(edge, G):
    // Implement a suitable algorithm to calculate betweenness for a given edge in the network
    // This can be done by computing the number of shortest paths passing through the edge
    
    // Return the betweenness score for the edge

function remove_edge_with_highest_betweenness(G):
    betweenness_scores = edge_betweenness_centrality(G)
    highest_betweenness = max(betweenness_scores.values())
    
    // If multiple edges have the same highest betweenness score, choose one at random
    highest_betweenness_edges = [edge for edge, score in betweenness_scores.items() if score == highest_betweenness]
    edge_to_remove = random.choice(highest_betweenness_edges)
    
    G.remove_edge(edge_to_remove)
    
    return G

function find_communities(G):
    communities = []  // List to store the identified communities
    
    while G has edges:
        G = remove_edge_with_highest_betweenness(G)
        components = get_connected_components(G)
        communities.append(components)
    
    return communities

function get_connected_components(G):
    // Implement a suitable algorithm to find the connected components in a graph
    // This can be done using depth-first search or breadth-first search
    
    // Return a list of connected components in the graph

// Main program
social_network = create_social_network()  // Create or load the social network graph

detected_communities = find_communities(social_network)
print(detected_communities)


## 5. Louvain algorithm PSUEDOCODE:
Input: Social network graph G

function initialize_communities(G):
    communities = {}  // Dictionary to store the initial community assignments
    
    for node in G.nodes:
        communities[node] = node  // Each node initially belongs to its own community
    
    return communities

function calculate_modularity(G, communities):
    m = total_number_of_edges(G)
    Q = 0  // Modularity score
    
    for community in communities.values():
        nodes_in_community = get_nodes_in_community(G, community)
        edges_inside_community = count_edges_inside_community(G, nodes_in_community)
        degree_of_community = sum_degrees_of_nodes(G, nodes_in_community)
        
        Q += edges_inside_community / (2 * m) - (degree_of_community / (2 * m))**2
    
    return Q

function get_nodes_in_community(G, community):
    nodes_in_community = []
    
    for node, comm in communities.items():
        if comm == community:
            nodes_in_community.append(node)
    
    return nodes_in_community

function count_edges_inside_community(G, nodes_in_community):
    edges_inside_community = 0
    
    for node1 in nodes_in_community:
        for node2 in nodes_in_community:
            if G.has_edge(node1, node2):
                edges_inside_community += 1
    
    return edges_inside_community

function sum_degrees_of_nodes(G, nodes):
    degree_sum = 0
    
    for node in nodes:
        degree_sum += G.degree(node)
    
    return degree_sum

function optimize_modularity(G):
    communities = initialize_communities(G)
    current_modularity = calculate_modularity(G, communities)
    improved = True
    
    while improved:
        improved = False
        
        for node in G.nodes:
            current_community = communities[node]
            neighbors = get_neighbors(G, node)
            best_community = current_community
            max_deltaQ = 0
            
            for neighbor in neighbors:
                deltaQ = calculate_delta_modularity(G, communities, node, neighbor)
                
                if deltaQ > max_deltaQ:
                    best_community = communities[neighbor]
                    max_deltaQ = deltaQ
            
            if best_community != current_community:
                communities[node] = best_community
                improved = True
        
        if improved:
            communities = update_communities(communities)
            new_modularity = calculate_modularity(G, communities)
            
            if new_modularity > current_modularity:
                current_modularity = new_modularity
                improved = True
    
    return communities

function get_neighbors(G, node):
    return G.neighbors(node)

function calculate_delta_modularity(G, communities, node, neighbor):
    m = total_number_of_edges(G)
    current_community = communities[node]
    new_community = communities[neighbor]
    degree_of_node = G.degree(node)
    degree_of_neighbor = G.degree(neighbor)
    edges_inside_community = count_edges_inside_community(G, get_nodes_in_community(G, current_community))
    edges_inside_new_community = count_edges_inside_community(G, get_nodes_in_community(G, new_community))
    
    deltaQ = (2 / (2 * m)) * (edges_inside_new_community - edges_inside_community)
    deltaQ -= (degree_of_node / (2 * m)) * ((degree_of_neighbor - degree_of_node) + (degree_of_node * degree_of_node))
    
    return deltaQ

function update_communities(communities):
    new_communities = {}
    community_mapping = {}
    i = 0
    
    for node, community in communities.items():
       
## 6. DESCRIPTION OF DATASET:
The given dataset represents a network of groups and their connections. Here's a description of the dataset:

The dataset consists of 29 rows, where each row represents a connection between two groups.
The three columns in the dataset are "group1," "group2," and "weight."
The "group1" and "group2" columns represent the IDs or labels of the groups involved in the connection.
The "weight" column represents the weight or strength of the connection between the groups.In this case, there is a connection with a weight of 2 between group1 with ID 19292162 and group2 with ID 535553.

Similarly, the other rows represent connections between different groups with corresponding weights.

The dataset provides information about the connections and their weights, which can be utilized to analyze the structure and relationships within the network of groups.
