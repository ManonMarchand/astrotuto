# Astrotuto

To come : Short tuto for data analysis using python  for the astrophysics VP in Fribourg Uni

## Get astro papers and data

https://simbad.u-strasbg.fr/simbad/

https://ui.adsabs.harvard.edu/classic-form

## Photometry

### Ressources

- evolution of a cluster https://esahubble.org/videos/heic1211a/
- information on galactic globular clusters https://physics.mcmaster.ca/~harris/mwgc.dat

### Find reference stars in the field of view

Here is the example for the globular cluster M5. 

A famous star just close to the cluster is 5 Serpens. You'll need to find its AAVSO UID. To do so, go on : https://www.aavso.org/vsx/index.php?view=search.top and search this star star by its usual name, here *5 Ser*. Be sure to include the non-varaible and suspects in you search. 

![image](https://user-images.githubusercontent.com/16650466/156001860-2448dac7-14e8-4169-8744-05aef07ee072.png)

Now, you'll want to plot a photometric chart around this star. On the aavso website, https://www.aavso.org/ , use the *pick a star* tool 

![image](https://user-images.githubusercontent.com/16650466/156002327-73f173ed-1bc4-413d-b198-fbcd35f4661e.png)

And press create a finder chart.

![image](https://user-images.githubusercontent.com/16650466/156002728-c17bf854-b156-4034-8a12-7aa5839f84fe.png)

We can clearly find M5 in here and some stars have labels. They are non-variable and calibrated stars that can be used for your H-R diagram. By pressing the "photometry table for this chart" link, you'll obtain the B-V and V values for the stars corresponding to the labels on the chart. 

![image](https://user-images.githubusercontent.com/16650466/156003327-3f276ef7-2219-4496-8606-b0460a2d4997.png)



### Queries from Simbad : 

- https://simbad.u-strasbg.fr/simbad/sim-tap
- tap is a lot like SQL
- this example query gives all objects within 16 arcmin (0.26°) around the center of M92 where the B and V data exists. 

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

- note : B is for blue and V for visual (which happens to be green)


## Spectroscopy

## Radial intensity of luminosity in galaxies

http://spiff.rit.edu/classes/phys443/lectures/gal_1/petro/petro.html

## Image analysis

Python libraries : 
- scikit image
- astropy
- numpy
- scipy

## Open a fit file 

--> astropy
