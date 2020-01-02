# Skeleton-based Line Detection {#line-detection-skeletonization status=ready}

## Overview

The [segment-based line detection](#line_detection) pipeline available in the [`line_detection`](https://github.com/duckietown/dt-core/tree/master19/packages/line_detector) package provides a nice and compact representation of the different lines in Duckietown for the downstream state estimation. Although it convenient, this representation does not handle turns well, and can suffer from many false positives. To improve these detections, we would like to compress the lines in a representation that preserves as much information as possible.

In order to retain the morphology of the lines, we can extract their **skeletons**. In [](#fig:pipeline-overview), we show the overall pipeline to detect, filter and compress the lines. The output of this pipeline is a set of skeletons that correspond to the different lines.

<figure id="fig:pipeline-overview">
    <figcaption>Pipeline overview</figcaption>
    <img style="width:100%" src="figures/pipeline-overview.png"/>
</figure>

## Preprocessing

Following the segment-based line detection pipeline, the raw camera input is first normalized to be as invariance to lighting changes as possible. See [](#anti-instagram-ttic) for details on this normalization. The image is then resized an cropped to remove the upper part of the image, which is unlikely to contain relevant information for line detection.

<figure class="flow-subfigures">  
    <figcaption>Preprocessing</figcaption>
    <figure>
        <figcaption>Normalized image</figcaption>
        <img style='width:184px' src="raw_image.png"/>
    </figure>
    <figure>
        <figcaption>Resized &amp; cropped image</figcaption>
        <img style='width:120px' src="preprocessing.png"/>
    </figure>
</figure>

## HSV Thresholding {#skeletonization-hsv-thresholding}

The only relevant colors we need to detect here are white (side lines), yellow (middle line) and red (stop line). The colors of interest can be easily and accurately filtered out using the HSV color space (Hue-Saturation-Value). In particular, *yellow* and *red* can be detected using a threshold on the *Hue*, and *white* using a threshold of the *Value*. This filtering step, along with its parameters, is identical to the segment-based line detection (see [](#line_detection)).

<figure>
    <figcaption>Color filtering in HSV color space</figcaption>
    <img style="width:180px" src="figures/raw_detections.png"/>
</figure>

## Road detection {#skeletonization-road-detection}

The HSV thresholding usually returns a large number of detections, many of which are spurious for our purposes (e.g. duckies on the side of the road, white walls). In order to minimize the amount of false positives, and to reduce the burden on the lane filtering, we can first apply a filter to remove these aberrant detections. For that, we need to detect the road to filter out outliers.

<figure class="flow-subfigures">  
    <figcaption>Road detection</figcaption>
    <figure>
        <figcaption>Color thresholding</figcaption>
        <img style='width:120px' src="black_mask.png"/>
    </figure>
    <figure>
        <figcaption>Opening</figcaption>
        <img style='width:120px' src="black_mask_dilated.png"/>
    </figure>
    <figure>
        <figcaption>Flood filling</figcaption>
        <img style='width:120px' src="road_mask.png"/>
    </figure>
</figure>

Note that since this is only used as part of the filter, the detection of the road can be coarse (e.g. maximizing the precision of our prediction). The detection of the road itself is not used directly for the line detection. It follows three steps:

 - First the color *black* is detected using **HSV thresholding**, similar to [](#skeletonization-hsv-thresholding). This mask is then combined with the detections of *yellow* and *red* found by HSV thresholding, to include the yellow and red lines which are part of the road. The corresponding binary mask, shown in Figure (a), contains a lot of noise. We also remove the upper part of the mask, in preparation for the next step.
 - We then apply a morphological transformation to this mask called **opening**: an erosion followed by a dilation using a relatively large kernel. This has the effect of removing noise and small connections between different regions; this is crucial to avoid "leaks" outside of the road when applying flood filling later. The resulting binary image is shown in Figure (b). This mask still contains regions which do not correspond to the road.
 - Finally, we apply **flood filling** on this binary mask, with an initial seed at the bottom of the image. This implicitly assumes that the duckiebot is on the road. This retains the only connected component corresponding to the road. The final component is then dilated with a small kernel to remove any artifact, and to prepare for [](#skeletonization-line-filtering). This is shown in Figure (c).

## Line Filtering {#skeletonization-line-filtering}

Using the coarse detection of the road, we can discard any detection which is not adjacent to the road; the remaining detections are likely the lines.

<figure class="flow-subfigures">  
    <figcaption>Line Filtering - Note that the first two images correspond to a single binary mask, and the detection of the road is displayed in red for visualization only.</figcaption>
    <figure>
        <figcaption>Road mask &amp; Detections</figcaption>
        <img style='width:120px' src="road_mask+detections.png"/>
    </figure>
    <figure>
        <figcaption>Road mask &amp; Inlier detections</figcaption>
        <img style='width:120px' src="road_mask+inlier_detections.png"/>
    </figure>
    <figure>
        <figcaption>Inlier detections</figcaption>
        <img style='width:120px' src="inlier_detections.png"/>
    </figure>
</figure>

The line filtering also follows three steps:

 - The road detection and the color detections found in [](#skeletonization-hsv-thresholding) are **combined together** in a single binary mask. This is shown in Figure (a). Note that the final dilation in [](#skeletonization-road-detection) allows the detected road mask to be connected to the white side lines.
 - In order to keep only the detections adjacent to the road, we apply a second **flood filling** step using an initial seed in the road. This has the effect of selecting all the components connected to the road. These are shown in Figure (b). Note that this is still a binary mask.
 - The binary mask is then used to **filter** the color detections. The filtered detections are shown in Figure (c).

## Skeletonization {#skeletonization-skeletonization}

The [skeletonization](https://homepages.inf.ed.ac.uk/rbf/HIPR2/skeleton.htm) is a step that removes most of the pixels in a binary image, while preserving the morphology and size of the original region. This returns the simplest pixel representation of the lines, reduced to a 1-pixel line. This representation is still in pixel space, instead of being segments, and can therefore handle curves. We use the skeletonization method described in [](#bib:lee1994skeletonization), initially introduced for 3D-surfaces, but adapted to two-dimensionial objects. The final skeletons are shown in [](#fig:skeletons).

<figure id="fig:skeletons">
    <figcaption>Skeletonization</figcaption>
    <img style="width:180px" src="figures/skeletons.png"/>
</figure>

Note that since skeletonization retains the morphology of the original masks, it is very sensible to artifacts. Notably, if the mask contains any hole, this will be apparent in the returned skeleton, as shown in this example (bottom right).

## Skeletons Clustering {#skeletonization-clustering}

The skeletons detected in [](#skeletonization-skeletonization) are still live in pixel space. Even though we have segmented the lines according to their color, it might be useful to cluster them further (e.g. to distinguish between the left and right white line). For clustering, we can use the simple [DBSCAN algorithm](https://en.wikipedia.org/wiki/DBSCAN). This can be easily implemented using a combination of standard operations: a dilation with a large kernel and connected component detection. The final clusters of skeletons are shown in [](#fig:clusters).

<figure id="fig:clusters">
    <figcaption>Clustering</figcaption>
    <img style="width:180px" src="figures/clusters.png"/>
</figure>
