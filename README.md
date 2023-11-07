# Stock_Portfolio_Simulation
Equity money management strategies are largely classified as either ‘active’ or ‘passive’. The most common passive strategy is that of “indexing” where the goal is to choose a portfolio that mirrors the movements of the broad market population or a market index. Such a portfolio is called an index fund. For example, the QQQ Index fund tracks the NASDAQ-100 index.

Constructing an index fund that tracks a specific broad market index could be done by simply purchasing all n stocks in the index, with the same weights as in the index. However, this approach is impractical (many small positions) and expensive (rebalancing costs may frequently be incurred, price response to trading). An index fund with m stocks, where m is substantially smaller than the size of the target population, n, seems desirable.

## Approach
In this project, two approaches will be compared.
* 2-Step Process
* MIP Process

### 2-Step Process
The binary decision variable y<sub>j</sub> indicates which stocks j from the index are present in the fund (y<sub>j</sub> = 1 if j is selected in the fund, 0 otherwise). For each stock in the index, i = 1, . . . , n, the binary decision variable x<sub>ij</sub> indicates which stock j in the index is the best representative of stock i (x<sub>ij</sub> = 1 if stock j in the index is the most similar stock i, 0 otherwise).

The first constraint selects exactly m stocks to be held in the fund. The second constraint imposes that each stock i has exactly one representative stock j in the index. The third constraint guarantees that stock i is best represented by stock j only if j is in the fund. The objective of the model maximize the similarity between the n stocks and their representatives in the fund.

Another way of thinking is that you have two sets. Set I has all stocks in the index. Set F has fund stocks. You want to create a link that maps elements of set I to elements of set F. The first constraint in the formulation above is equivalent to saying that not more than q elements of F can be mapped from set I. The second constraint suggests that each element in the set I will map to a single element in set F. The third constraint suggests that if an element from set I gets mapped to an element in set F, then the element of set F better be present in the fund. The binary constraint on x<sub>ij</sub> indicates if a link between element i in set I and element j in set F exists. The weight on that link is the correlation ρij between stock i in set I and stock j in set F. Many such mappings which satisfy the above constraints exist. Your objective gives you the best mapping. This is the basic idea of bipartite matching.

$$ 
\begin{align} 
\max_{x,y} \sum_{i=1}^{n} \sum_{j=1}^{n} \rho_{ij} x{ij} \\
s.t. \sum_{j=1}^{n} y_j = m \\
\sum_{j=1}^{n} x_{ij} = 1   for i = 1,2,3,....,n \\
x_{ij} <= y_j   for i,j = 1,2,3,....,n \\
x_{ij},y_j \in \{ 0,1 \}
\end{align}
$$

To get the portfolio weights you will try to match the returns of the index as closely as possible. Let’s call 𝑟𝑖𝑡 the return of stock i (where stock i is one of the chosen stocks from above) at time period t, 𝑞𝑡 the return of the index at time t, and 𝑤𝑖 the weight of stock i in the portfolio. You want to solve

$$ 
\begin{align} 
\min_{w} \sum_{t=1}^{T} | q_t - \sum_{i=1}^{m} w_i r_{it} | \\
s.t. \sum_{i=1}^{m} w_i = 1 \\
w_{i} >= 0
\end{align}
$$

### MIP Approach
This approach is similar to the above approach with the modification that the weights and the stock selections are modeled together using the bigM constraints.
