# Social Network Analysis - Gossip Spread

## Project Overview
This university project leverages Social Network Analysis (SNA) to explore the dynamics of gossip spread among a group of college students. The analysis focuses on both positive and negative gossip networks, examining key players, network size, and centrality measures. The project was conducted in RMarkdown (.rmd), and the HTML and Markdown (.md) output file is included for convenient viewing without running the code.

## Dataset
The original dataset used for this analysis is available on Mendeley: [Gossip Spread Dataset](https://data.mendeley.com/datasets/kpjjvg39k3/4).

Note on Data Preparation:
- The dataset used in this project was pre-processed in Excel before analysis in R.
- Pre-processing included minor data cleaning (e.g., filling in blank cells with 0).
- The R code in this repository uses this pre-processed version of the dataset.

For exact replication of results or to access the pre-processed dataset, please contact the repository owner.

## Project Structure

### Part 1: Data Description
- Convert both negative and positive gossip dataframes into `igraph` objects.
- Create visualizations for both networks.

### Part 2: Key Player Analysis (KPA)
- Perform Key Player Analysis for both negative and positive gossip networks.
- Generate visualizations to highlight key players.

### Part 3: Effective Network Size (ENS)
- Calculate the Effective Network Size for both negative and positive gossip networks.
- Visualize the results to understand network efficiency.

### Part 4: Centrality Measures
- Compute centrality measures (node degree and closeness) for both negative and positive gossip networks.

## How to Use
1. Clone the repository.
2. Open the RMarkdown file in your preferred R environment.
3. Run the code chunks sequentially to reproduce the analysis.
4. Alternatively, view the HTML file for a quick summary of results.

## Feedback
Your feedback is always welcome! Feel free to share any suggestions or improvements you might have.
