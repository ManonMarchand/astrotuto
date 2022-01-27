# Astrotuto

To come : Short tuto for data analysis using python  for the astrophysics VP in Fribourg Uni

## Photometry

### Ressources

- evolution of an old cluster https://esahubble.org/videos/heic1211a/

### Queries from Simbad : 

This query gives all objects within 16 arcmin (0.26°) around the center of M92 where the B and V data exists. 

```sql:
SELECT B, V, 
       main_id, ra, dec, 
       distance(POINT('ICRS', ra, dec), point('ICRS',259.2807916666667, 43.13594444444444)) as dist 
FROM allfluxes 
JOIN ident USING(oidref) 
JOIN basic on allfluxes.oidref = basic.oid
WHERE (CONTAINS(POINT('ICRS', ra, dec), CIRCLE('ICRS', 259.2807916666667, 43.13594444444444, 0.26666666666666666)) = 1) 
AND (B IS NOT NULL) 
AND (V IS NOT NULL)
ORDER BY dist
```


## Spectroscopy

## Image analysis
