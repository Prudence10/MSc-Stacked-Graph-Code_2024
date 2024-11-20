# MSc-Stacked-Graph-Code_2024
# -*- coding: utf-8 -*-
"""
Created on Sat Nov 16 17:50:31 2024

@author: prude
"""

import pandas as pd
import matplotlib.pyplot as plt

# Load and preprocess the data
df = pd.read_excel('2 AMR 2023-2024.xlsx')
# Strip spaces from column names
df.columns = df.columns.str.strip()  

# Melt the data
df_melted = df.melt(id_vars=['DATE', 'SITE'], 
                    value_vars=['KPC', 'OXA-23', 'OXA-51', 'OXA-48', 'VIM', 'NDM', 'SHV', 'CTX-M', 'TEM'],
                    var_name='Gene', value_name='Presence')

# Convert DATE to datetime and sort
df_melted['DATE'] = pd.to_datetime(df_melted['DATE'], format='%d-%b-%y')
# Filter to show only presence
df_melted = df_melted[df_melted['Presence'] == 1]  

# Pivot the data to get gene presence over time
df_pivot = df_melted.pivot_table(index='DATE', columns='Gene', values='Presence', aggfunc='sum', fill_value=0)

# Increase figure size for better spacing
plt.figure(figsize=(14, 8))

# Plot a stacked bar plot
ax = df_pivot.plot.bar(stacked=True, figsize=(14, 8), colormap='tab20')

# Set titles and labels
plt.title("AMR Gene Prevalence Over Time (July 2023 - July 2024)", fontsize=16, pad=20)
plt.xlabel("Date", fontsize=12, labelpad=20)
plt.ylabel("Gene Presence Count", fontsize=12, labelpad=20)

# Adjust legend position to avoid overlap with the graph
plt.legend(title='Gene', bbox_to_anchor=(1.05, 1), loc='upper left')

# Rotate x-axis labels for better visibility
plt.xticks(rotation=45, ha='right')

# Ensure everything fits without overlapping
plt.tight_layout(pad=4.0)

# Show the plot
plt.show()
