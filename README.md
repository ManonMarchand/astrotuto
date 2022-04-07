# Astrotuto

For this lecture, you'll analyse data using python. Don't hesitate to come to my office if you're not familiar with it or if you encounter struggles to install it on your machine. 
My advice is to use notebooks for image analysis.

# Table of contents

## Basics of astro data manipulation

* [Find your pictures](#find-your-pictures)
* [Open a fit file](#open-a-fit-file)
* [Get a lit of all images](#get-a-list-of-all-images)
* [Look at one image](#look-at-one-image)
* [Plot an histogram](#plot-an-histogram)

## Bibliography

* [Example query for astro sql on Simbad](#queries-from-simbad)
* [Where to get astro papers and data](#get-astro-papers-and-data)


## Specific informations for different projects

* [Photometry](#photometry)
* [Spectroscopy](#spectroscopy)

---------------------

# Data manipulation


## Find your pictures

```python
import glob

list_all_darks = glob.glob('./**/*DARK.FIT', recursive=True)

```
Here, we use the library glob to list all the paths to files nested in the current working directory and that end with 'DARK.FIT'

## Open a fit file

This time `list_imgs` (also obtained with glob) contains a list of paths to light frames.

```python
from astropy.io import fits

fit_file = fits.open(list_imgs[1])

header = fit_file[0].header
img = fit_file[0].data

```
Each fit file contains a header with information about the image and the image itself. In this case, the header looks like this : 

![image](https://user-images.githubusercontent.com/16650466/156556817-b640729d-4224-4704-ae7d-a1ff5713e187.png)

And the image itself is a 2D array

![image](https://user-images.githubusercontent.com/16650466/156556968-93009707-130f-44e5-86b4-32322c12c2d1.png)


## Get a list of all images

A practical way to work with several files is to create a 3D array where the third dimension is the number of the image in the list.

```python
all_darks = [] # create an empty array
for fit in list_all_darks: # browse through the paths to dark fits
  with fits.open(fit) as hdu: # for header/data unit
    all_darks.append(np.array(hdu[0].data, dtype=float))
```

## Look at one image

```python
import matplotlib.pyplot as plt # it's a library to create plots, you can use an other one if you prefer
img = all_darks[0] #we take the first image in our list of darks

fig = plt.figure(figsize=(10,10)) # create a figure
plt.imshow(img, cmap='gray') # plot with a grayscale colormap
plt.colorbar() # add a color bar to see the corresponding values

```

![image](https://user-images.githubusercontent.com/16650466/156561950-73ca67c7-7336-4064-b7f4-c4725e940f32.png)


## Plot an histogram

To see more details in this dark image, we can plot its histogram. 

```python
plt.hist(img.flatten(), 200)  # you can change the number 200 here, it just defines how coarse your histogram will be
plt.xlabel('value of pixel');
```

![image](https://user-images.githubusercontent.com/16650466/156560390-4c175ff9-b2f2-453c-8e88-bc2a386074f1.png)

To get to the bottom of what's happening around 2000 that could stretch the scale like this, we can look at some statistics : 

```python
import numpy as np

print('mean : ', np.mean(img))
print('median :', np.median(img))
print('min :', np.min(img))
print('max :', np.max(img))
```

![image](https://user-images.githubusercontent.com/16650466/156561434-a1b91249-d380-4f74-8483-f5af90651fc4.png)


Here we have way more information : most of the noise in around 1400 while a very few pixels have a value around 2100 (this is what stretches the scale here). Those high value pixels are most likely dead pixels of the CCD camera that will always stay white. This artefact will disapear when the darks will be substracted from the light frames.





## Get astro papers and data

https://simbad.u-strasbg.fr/simbad/

https://ui.adsabs.harvard.edu/classic-form

## Queries from Simbad

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






## Spectroscopy

## Radial intensity of luminosity in galaxies

http://spiff.rit.edu/classes/phys443/lectures/gal_1/petro/petro.html

## Possible projects ?

https://oro.open.ac.uk/69186/1/69186.pdf



