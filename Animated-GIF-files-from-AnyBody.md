## How to create animated GIF files from AnyBody

Requirements: [ImageMagick](http://www.imagemagick.org/) must be installed and added to the system path

Change the background color of the model view to some distinct color (eg yellow). This will allow ImageMagick to create a transparent background in the final animation. Note however, that this will not work well if some parts of your model are semi-transparent. In that case it is better to use a white background or what ever matches the background color where the figure will be used.

Use the record button in Model View to save a number of image files.

Open a command prompt where the images are saved and type the following command:
```
     convert -delay 10 -dispose previous  -loop 0  *.bmp  -transparent yellow -coalesce animation.gif
```

The command assumes that image files are saved as .bmp files with a full yellow background in the model view.

![animation](https://cloud.githubusercontent.com/assets/22542671/20790319/2dccf012-b7b8-11e6-9d8d-42ffa2b98b4e.gif)