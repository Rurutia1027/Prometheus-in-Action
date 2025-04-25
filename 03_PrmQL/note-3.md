# Modifiers 

## Offset Modifer 
- To get historic data use an **offset modifier** after the label matching. 

```text 
node_memory_Active_bytes{instance="node01"} offset  5m 
# this gonna return the selector & modifiers filtered resutls 5 mintues ago 


# fetch data 5 days ago 
node_memory_Active_bytes{instance="node01"} offset 5d 

# fetch data 2 weeks ago 
node_memory_Active_bytes{instance="node01"} offset 2w 


# fetch data 1.5 h ago 
node_memory_Active_bytes{instance="node01"} offset 1h30m 


# To go back to a specific point in time use the @modifier -> this represent the specific unix timestamp value like: 

node_memory_Active_bytes{instance="node01"} @1663265188 
# 1663265188 is timestamp in Unix timestamp 
```

## Combination of Offset Modifier & @modifier 
```text

#  offset modifier & @modifier does not order sensitive 
node_memory_Active_bytes{instance="node01"} @1663265188 offset 5m 
```