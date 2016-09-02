
# Smarter Images

Images now account for almost two-thirds of the total weight of an average web page^[[http://httparchive.org/interesting.php#bytesperpage](http://httparchive.org/interesting.php#bytesperpage)]. They are an important part of an engaging web experience, but we can be smarter about how we use them.

The convergence of high-density displays and mobile, low-bandwidth computing puts two competing interests at odds. Your site performs better-particularly on spotty mobile networks---when images are smaller, but looks worse when viewed on high---resolution screens. And some of the highest resolution screens are found on tablets and smartphones, which often have unpredictable internet connections.

How do you balance these competing interests?

## Picking the Right Format

Different image formats work better for different types of graphics.

PNG is a lossless image format, so it keeps graphics sharp and crisp (as opposed to the lossy JPG format). For icons and simple images with clean lines, they can actually be more lightweight than JPGs.

But for photos and images with lots of visual noise, JPGs will be much smaller in size with comparable quality.

## Picking the right JPG Format

The JPG actually has multiple formats. The most common on the web is the baseline JPG. Baseline JPGs start rendering at the top, and build down as they go.

An alternative format that was popular a decade ago and is seeing a bit of a comeback is the progressive JPG. Progressive JPGs build in layers. Initially, the full image in low resolution is displayed, and as the image renders, it becomes increasingly crisp and clear.

![Baseline vs. Progressive JPGs](img/jpg-progressive.jpg)

While progressive JPGs are typically a little smaller in size than baseline JPGs, their real advantage is that they appear faster to the user because they display more of an image at once. And on smaller screens, the lack of clarity on initial renders may not even be as noticeable.

While all browsers display progressive JPGs, some browsers do a better job than others^[[http://calendar.perfplanet.com/2012/progressive-jpegs-a-new-best-practice/](http://calendar.perfplanet.com/2012/progressive-jpegs-a-new-best-practice/)]. For "non-supporting" browsers, the entire progressive JPG needs to download before it can be displayed, resulting in a worse experience than a baseline JPG.

### Tools and techniques for creating progressive JPGs

- ImageOptim^[[https://imageoptim.com/](https://imageoptim.com/)] is a Mac-only desktop app
- b64.io^[[http://b64.io](http://b64.io)] is a web app

## Smush your images

The metadata that photographs include—--timestamps, color profiles, and such—--can add quite a bit of weight. Smushing is the process of removing that metadata, and it can reduce the size of an image by more than 25-percent.

Both ImageOptim and b64.io also smush PNGs, JPGs, and GIFs.

## Compress your JPGs

A high-quality photo can weigh upwards of 1mb or more. By compressing photos, you can reduce them down to a fraction of their original size while maintaining image quality.

![A massive reduction in file size from compression](img/jpg-compressed.jpg)

A JPG compression rate of 70 is considered high-quality for the web. WordPress uses a compression rate of 90 when it resizes your images.

This means that you could optimize a photo, upload to WordPress, and come out with a photo that's smaller in dimension but larger in weight than the one you uploaded.

In version 4.5, WordPress increased their compression rate to 82, which is better, but still not aggressive enough for the web.

Image Compress and Sharpen^[[https://github.com/cferdinandi/gmt-image-compress-and-sharpen](https://github.com/cferdinandi/gmt-image-compress-and-sharpen)] is a plugin I maintain on GitHub that lets you adjust the default compression rate in WordPress. It also converts JPGs to the progressive format.

If you'd like to recompress a bunch of existing images on your site, you can install one of the many thumbnail recreation plugins that exist. They'll recreate all of your resized images with the new compression settings.