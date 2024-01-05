# Scientific Color Maps

## This repository

This is a test environment for integration of scientific color maps into mathematica.
The function is integrated in [QMRITools](https://github.com/mfroeling/QMRITools).

This is still work in progress. I would be grateful if any of the coders here have some thoughts on how to improve upon what i did to get custom colormaps into Mathematica.

## Why these color maps

Back in 2018 Fabio Crameri proposed some good color maps for scientific visualization and recently V8 was released.

Crameri, F. (2018). Scientific colour maps. Zenodo. [doi](http://doi.org/10.5281/zenodo.1243862)
Crameri, F. (2018), Geodynamic diagnostics, scientific visualisation and StagLab 3.0, Geosci. Model Dev., 11, 2541-2562, [doi](https://doi.org/10.5194/gmd-11-2541-2018)
Crameri, F., G.E. Shephard, and P.J. Heron (2020), The misuse of colour in science communication, Nature Communications, 11, 5444. [doi](https://www.nature.com/articles/s41467-020-19160-7)

In the Readme the Mathematica integration is given by this code which works but needs to run every time and only one color function at a time.

```text
ColorMapSuitePath = "/Path/To/ColourMapSuite/";
ColorMapSuite[name_String] := ColorMapSuite[name, -1]
ColorMapSuite[name_String, el_] := With[{
list = Transpose@{Subdivide[0, 1, 255], RGBColor @@@ First@Import[
    ColorMapSuitePath <> "/" <> name <> "/" <> name <> ".mat"]}},
    Blend[list, {##}[[el]]] &
]
```

## Load all color maps

It was on my list for a long time but i have written some code that imports the color maps into mathematica but also integrates them in the ColorData functionality based on the following discussions from on stackexchange:

- https://mathematica.stackexchange.com/questions/57885/is-it-possible-to-insert-new-colour-schemes-into-colordata/57893#57893
- https://mathematica.stackexchange.com/questions/54648/can-you-make-the-new-choose-color-scheme-helper-larger
- https://mathematica.stackexchange.com/questions/255929/what-is-the-underlying-interpolation-in-brightbands-darkbands-color-scheme

Which is a bit "hacky" by inserting things into the DataPacletsColorDataDump but works. Regrettably, I could not find any nicer solutions for now to permanently store custom color functions in Mathematica. So i went on this path. The function i came up with is implemented in `ScientificColourMaps.nb` and `ScientificColourMaps.wl`.

This function adds all the functions to the ColorData functionality (but not the front end color picker). The following groups are added.

- SequentialGradients
- SequentialGradientsDiscrete
- SequentialGradientsCategorical
- DivergingGradients
- DivergingGradientsDiscrete
- MultiSequentialGradients
- MultiSequentialGradientsDiscrete
- CyclicGradients
- CyclicGradientsDiscrete

In `ScientificColourMaps.nb` there are also some test and demos to view all the color schems.

![all colors](https://github.com/mfroeling/ScientificColourMaps/blob/main/images/examples.png)

It all feels a bit cumbersome how it all worked out and the definitions of the color functions are a bit messy. I'm curious if anyone has some tips and tricks regarding color functions for a more "clean" integration before i start making this into a repository function and/or paclet since there are some open issues, for example:

- I dont have a clear idea how to make ColorData[{"Acton","Reverse"}] or ColorData[{"Acton",{0,10}}] work.
- Is hacking into the DataPacletsColorDataDump` realy the best idea.

![SequentialGradients](https://github.com/mfroeling/ScientificColourMaps/blob/main/images/SequentialGradients.jpg)
![DivergingGradients](https://github.com/mfroeling/ScientificColourMaps/blob/main/images/DivergingGradients.jpg)
![MultiSequentialGradients](https://github.com/mfroeling/ScientificColourMaps/blob/main/images/MultiSequentialGradients.jpg)
![CyclicGradients](https://github.com/mfroeling/ScientificColourMaps/blob/main/images/CyclicGradients.jpg)
