### set specific columns of empty dataframe to specific columns from other dataframe
```
df2[['date','program','alarm','var2']] = gdf[['date','monitorprogram','monitorvar1','monitorvar2']]
```
