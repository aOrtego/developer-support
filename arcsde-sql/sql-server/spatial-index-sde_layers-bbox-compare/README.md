spatial-index-sde_layers-bbox-compare.sql
===================
SQL Server - TSQL

Compare the bounding box values of each SQL Server Geometry spatial index tessellation to the Min/Max X,Y values in the SDE_Layers table.

----------


REQUIRED:
Enterprise SQL Server geodatabase.
Existing feature classes stored in either SQL Server Geometry or Geography spatial type.

```sql
SELECT l.owner AS sde_layers_owner,
	l.table_name AS sde_layers_name,
	si.name as spatial_index_name, 
	l.minx AS sde_layers_minx, 
	l.miny AS sde_layers_miny, 
	l.maxx AS sde_layers_maxx, 
	l.maxy AS sde_layers_maxy,
	'' AS '-',
	sit.bounding_box_xmin AS si_minx,
	sit.bounding_box_ymin AS si_miny,
	sit.bounding_box_xmax AS si_maxx,
	sit.bounding_box_xmax AS si_maxy
FROM sde.SDE_layers l
INNER JOIN sys.objects t
	ON l.table_name = t.name
INNER JOIN sys.indexes si
	ON t.object_id = si.object_id
INNER JOIN sys.spatial_index_tessellations sit
	ON si.object_id = sit.object_id
WHERE si.type = 4
ORDER BY t.name
```
DOCUMENTATION:
[Spatial Indexes Overview][1]
[System Tables of a Geodatabase in SQL Server][2]
[1]:https://msdn.microsoft.com/en-us/library/bb895265.aspx
[2]:http://resources.arcgis.com/en/help/main/10.2/index.html#//002q00000080000000