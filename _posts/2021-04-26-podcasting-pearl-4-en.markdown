---
title: 'Podcasting Pearl #4: Scale your analytics on the Clickhouse Data Warehouse.'
date: 2021-04-26 02:02:36 Z
categories:
- podcast
layout: post
lang: en
ref: pearl4
---

ClickHouse: open-source, column oriented DB  engine.

<iframe src="https://open.spotify.com/embed/episode/7zak3WAkcXZ0dlZaHvVLS4" width="100%" style="max-width:660px" height="152" frameBorder="0" allowtransparency="true" allow="encrypted-media"></iframe>
<iframe src="https://embed.podcasts.apple.com/us/podcast/scale-your-analytics-on-the-clickhouse-data-warehouse/id1193040557?i=1000443943650&amp;itsct=podcast_box_player&amp;itscg=30200&amp;ls=1&amp;theme=light" height="175px" frameborder="0" sandbox="allow-forms allow-popups allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="autoplay *; encrypted-media *;" style="width: 100%; max-width: 660px; overflow: hidden; border-radius: 10px; background: transparent;"></iframe>

**Disclaimer**:
Some technical details are outdated in this podcast, but the discussion is still very relevant.

**Principles:**
- created by Yandex for equivalent of Google Analytics: OLAP use cases. 
- columnar: compress data along columns. To add data: decompress, merge sort, compress. This for all columns.
 
**Consequences of columnar design:**  
- does not like deletes: requires to decompress chunks, sometimes to copy big amount of data.
- inserting row by row is will have bad performance
- accessing single row is not effective


**Inserting capabilities:**  
- batch load 
- HTTP API, but tCP with typed column is better
- CLI (great)
- read from Kafka
- read from datalake
- read other DBs (MySQL, PostGre)

Fact table in ClickHouse OLAP, dimension table in SQL for easy update (OLTP).

**Nice features:**  
- array management (nested columns)
- aggregation in real-time
- approximate unique (sketches, hyperloglog like)
- approximate joins 
- SQL based ttl: allows to change storage type based on time

**Modelling:**  
Easy thanks to schema evolution.

1. 
    - partition key
    - primary sort order
    - load everything as string
    
2. 
    - update types
    - codex: compression
    - low cardinality strings (lookup table for strings with few different values)
    
3. 
    - store data in arrays
    - materialize columns
    - ETL to ELT paradigm
    
**Installation:**  
Easy to install, easy to manage (for a distributed OLAP DB).
- focus on infrastructure integration before performance
- bare metal >> virtualized for performance
- works quite well on Kubernetes (requires some tweaking)

Security and access control and  was not very mature in 2019.

**Vision of the future:**  
OLAP and AI are two separate worlds: this will change. ML will be embedded into the DBs.  
- keeps compute close to the data --> cost + performance
- same language for data gathering and processing  --> less errors

    
