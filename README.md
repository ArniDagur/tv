
**tv** ("textview") is a small tool to quickly view high-resolution multi-band imagery directly in your terminal. It was designed for working with (very large) satellite imagery data over a low-bandwidth connection. For example, you can directly visualise a Himawari 8 (11K x 11K pixel) image of the Earth directly from its URL:

<img src="https://github.com/daleroberts/tv/blob/master/docs/earth.png" width="800">

It is built upon the wonderful [GDAL](http://www.gdal.org) library so it is able to load a large variety of image formats (e.g., PNG, Jpeg, NetCDF) and subsample the image as it reads from disk so it can handle very large files quickly. It has the ability to read filenames (or URLs) from `stdin` and load files directly from URLs without writing locally to disk. Command line options are styled after `gdal_translate` such as:

  *  `-b` to specify the bands (and ordering) to use,
  * `-srcwin xoff yoff xsize ysize` to view a subset of the image,
  * `-r` to specify the subsampling algorithm (`nearest`, `bilinear`, `cubic`, `cubicspline`, `lanczos`, `average`, `mode`).

**tv** is completely implemented in Python 3 using only Numpy and GDAL 2.0.

My rendering approach is different from other tools such as [hiptext](https://github.com/jart/hiptext) as I use more unicode characters and [true color](https://gist.github.com/XVilka/8346728). This means that you get **amazingly better results** as long as your terminal and font supports it. Here is a comparison between **hiptext** (left) and **tv** (right) using their [benchmark image](https://github.com/jart/hiptext/blob/master/obama.jpg) of Barack Obama using the standard MacOS font 'Menlo Regular' at size 11 in [iTerm 2.0](https://www.iterm2.com/index.html).

<img src="https://github.com/daleroberts/tv/blob/master/docs/hiptext_obama.png" width="400">
<img src="https://github.com/daleroberts/tv/blob/master/docs/tv_obama.png" width="400">

You can easily zoom in to get better detail or make the output smaller.

<img src="https://github.com/daleroberts/tv/blob/master/docs/obama_eye.png" width="800">

You can quickly view very large files over low-bandwidth connections (e.g., mobile). For example, a 46GB single-band 176000 x 140000 pixel image using nearest neighbour subsampling located on the [raijin supercomputer](http://www.nci.org.au).

<img src="https://github.com/daleroberts/tv/blob/master/docs/bigfile.png" width="800">

It can detect URLs on the standard input which allows you to use it in combination with other tools such as [landsat-util](https://github.com/developmentseed/landsat-util) to quickly visualise thumbnails before you perform a full download.

<img src="https://github.com/daleroberts/tv/blob/master/docs/urls_stdin.png" width="700">

You can directly give a URL on the command line.

<img src="https://github.com/daleroberts/tv/blob/master/docs/urls.png" width="750">

If you have a image with more than 3 bands (channels), you can specify the ordering and the bands that you would like to load into the RGB channels.

<img src="https://github.com/daleroberts/tv/blob/master/docs/landsat_bands.png" width="800">

It's a bit rough but if you really want to throttle back the number of unicode characters used, you can do it with a command line option. The following example shows how to use only the block character or a half-height character as well. This helps if your font doesn't support certain unicode characters.

<img src="https://github.com/daleroberts/tv/blob/master/docs/unicodes.png" width="800">

Using a GNU parallel, you can do silly things like create an animation from the Himawari-8 satellite.

```
parallel --willcite --tty --header : tv -w 60 -urls http://himawari8-dl.nict.go.jp/himawari8/img/D531106/thumbnail/550/2016/06/{dy}/{hr}{tenmin}000_0_0.png  ::: dy 06 ::: hr 06 ::: tenmin {0..5}
```

<img src="https://github.com/daleroberts/tv/blob/master/docs/anim.gif" width="600">
