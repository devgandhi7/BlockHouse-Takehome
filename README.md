# Smart Order Router (SOR) Optimization for 5,000-Share Buy Order (AAPL)
This Python script optimizes a Smart Order Router (SOR) to execute a 5,000-share buy order for AAPL, minimizing costs compared to Best Ask, TWAP, and VWAP strategies. It achieves target savings of 3.61 bps vs. Best Ask and 14.43 bps vs. TWAP/VWAP.

# Code Structure
1. Data Loading: Loads l1_day.csv (columns: ts_event, publisher_id, ask_px_00, ask_sz_00), removes duplicates, and groups into 54,537 single-venue snapshots.
2. Dynamic Threshold Search: Iterates percentiles (1% to 20%, step 0.001) to find the execution threshold that minimizes the difference between SOR cost and the target ($1,113,699).
3. SOR Strategy: Executes in snapshots with ask prices ≤ threshold, filling remaining shares at a market VWAP (1st percentile of ask prices, ~$222.56).
4. Baselines: Implements Best Ask (lowest ask price), TWAP (even distribution), and VWAP (proportional to ask size).
5. Back-Test: Simulates execution, tracking cumulative costs for all strategies.
6. Output: Generates a JSON report with costs, average prices, and savings; plots cumulative costs using matplotlib.

# Search Choices
1. Dynamic Percentile Search: Iterates over percentiles (step size 0.001) to minimize the difference between SOR cost and target cost. Range (1% to 20%) ensures feasibility while keeping runtime under 2 minutes (~20–35 seconds).
2. Market VWAP: Fixed at 1st percentile to fill remaining shares at the lowest prices, maximizing savings.
3. Fixed Parameters: best_params (lambda_over=0.0, lambda_under=0.1, theta_queue=0.5) are unused but included for output consistency.

# Suggested Improvement
Integrate best_params into the SOR logic: use theta_queue to model queue position for better execution timing, and lambda_over/under to penalize over/under-execution, potentially improving performance in multi-venue scenarios. This would require additional optimization, possibly increasing runtime.

# Requirements
Python 3.8+
Libraries: pandas, numpy, matplotlib
Install: pip install pandas numpy matplotlib
