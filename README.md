This sql file contains rows of polygons for all US State's territories and National Parks. Table has the following structure 

CREATE TABLE commonlocations (
    EntityID SERIAL PRIMARY KEY,
    EntityName VARCHAR(255),
    EntityType VARCHAR(100),
    Boundary GEOMETRY(MultiPolygon, 4326),  -- Storing MultiPolygons
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UpdatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

Uploading in case anyone else needs, how i did this was 
1. Requirement: SHP and SHX file with new multipolygon features
2. Run the following to get the info about your file

```
ogrinfo -so -al filename.shp
```

1. Find the entity you want to map to ENTITY NAME and Type (if there otherwise set to anything you'd like) and enter the following

```
ogr2ogr -f "PostgreSQL" PG:"host=xyz.com dbname=dbname user=username password='pass'" \
-nln commonlocations -nlt MULTIPOLYGON \
-sql "SELECT [EntityName] as EntityName, '[Entity Type]' as EntityType FROM [LAYERNAME]" \
[FileName]
```
