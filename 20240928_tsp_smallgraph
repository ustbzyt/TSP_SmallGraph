# Version #1
class Graph_Advanced(Graph):
    def cache_edge_weights(self):
        """
        Precomputes and caches the weights of all edges in the graph.
        This avoids repeated calls to _get_edge_weight during TSP calculations.
        
        Returns:
        - A dictionary where each key is a tuple (src, dest) representing an edge, and the value is the weight of that edge.
        """
        edge_cache = {}
        
        for src in self.graph:
            for dest in self.graph:
                if src != dest:  # No need to compute distance from a node to itself
                    edge_cache[(src, dest)] = self._get_edge_weight(src, dest)
        
        return edge_cache

    def calculate_tour_distance(self, tour, edge_cache, current_best=None):
        """
        Given a list of vertices representing a tour, calculate the total distance of the tour using cached edge weights.
        The tour must return to the starting node.

        Parameters:
        - tour: A list of vertices representing a tour.
        - edge_cache: A dictionary containing cached edge weights for the graph.
        - current_best: The current shortest distance found (optional). If provided, stops calculation early if distance exceeds this value.

        Returns:
        - The total distance of the tour, including the return to the start.
        - Returns float('inf') if the tour is invalid (e.g., if an edge is missing).
        """
        total_distance = 0

        # Loop through the tour and calculate the total distance
        for i in range(len(tour) - 1):
            # Get the edge weight from the cache
            weight = edge_cache.get((tour[i], tour[i+1]), float('inf'))
            
            if weight == float('inf'):
                # If there's no valid edge, return infinity to indicate an invalid tour
                return float('inf')

            total_distance += weight

            # Early exit if the current total distance exceeds the current best
            if current_best is not None and total_distance >= current_best:
                return float('inf')

        # Add the distance to return to the starting node
        return_trip_weight = edge_cache.get((tour[-1], tour[0]), float('inf'))
        if return_trip_weight == float('inf'):
            # If no return trip is possible, return infinity
            return float('inf')

        total_distance += return_trip_weight

        # Return the total distance of the valid tour
        return total_distance

    def tsp_small_graph(self, start_vertex):
        """
        Solves the Travelling Salesman Problem (TSP) for a small (~10 node) complete graph using brute force.
        
        Parameters:
        - start_vertex: The starting vertex for the TSP.

        Returns:
        - A tuple (dist, path) where:
          - dist is the shortest possible tour distance.
          - path is the list of vertices representing the optimal tour.
        """
        # Cache edge weights to avoid repeated calls to _get_edge_weight
        edge_cache = self.cache_edge_weights()

        # List of all vertices except the start_vertex
        vertices = list(self.graph.keys())
        vertices.remove(start_vertex)  # Remove the start vertex from the list to generate permutations
        
        # Generate all permutations of the remaining vertices
        all_permutations = itertools.permutations(vertices)
        
        dist = float('inf')
        path = None
        
        # Try each permutation, calculate the tour distance, and find the shortest one
        for perm in all_permutations:
            # Add the start vertex to the beginning of the permutation to form a tour
            tour = [start_vertex] + list(perm)
            
            # Calculate the distance of this tour using the edge cache
            current_distance = self.calculate_tour_distance(tour, edge_cache, current_best=dist)

            # Update the minimum distance and best path if necessary
            if current_distance < dist:
                dist = current_distance
                path = tour

        return dist, path
