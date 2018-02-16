{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "*Finding Lane Lines on the Road** \n",
    "\n",
    "## Project writeup\n",
    "\n",
    "---\n",
    "\n",
    "**Finding Lane Lines on the Road**\n",
    "\n",
    "The goals / steps of this project are the following:\n",
    "* Make a pipeline that finds lane lines on the road\n",
    "* Reflect on my work in a written report\n",
    "\n",
    "\n",
    "[//]: # (Image References)\n",
    "\n",
    "[image1]: ./examples/grayscale.jpg \"Grayscale\"\n",
    "\n",
    "---\n",
    "\n",
    "### Reflection\n",
    "\n",
    "### 1. Pipeline: explain how you modified the draw_lines() function.\n",
    "\n",
    "Inorder to detect lanes on road using opencv, I followed the below steps in this project.\n",
    "\n",
    "* Covert original image into gray scale image. \n",
    "* Apply gaussian filtering to remove any noise\n",
    "* Detect egses using canny edge detection. Set high_thresold first and play around with low_threshold to get appropiate results.\n",
    "* Apply mask for region of interest after edge detection to remove the edge due to masking RoI. Choose the vertices based on sample test image as the region of interest is fixed.\n",
    "* Apply Hough transform to detects line segments in the image.\n",
    "  Threshold : Adjust threshold to detect pts on line segment. Smaller value of threshold detects smaller line segment as well.\n",
    "  min_line_len : Select minimum number of points that make up the line segmet. Smaller more finer\n",
    "  \n",
    "\n",
    "Final part of the the project is to draw lines on the partial detected lanes. The main idea behind drawing lanes is to find the slope of the lane and pick on point so that a line segment can me drawn. This line segment needed to be close to the actual lane.\n",
    "\n",
    "Inorder to find the optimal slope(according to me), I started of binning slopes of different line segments obtained at the output of Hough transform. Then I picked optimal slope as the bin with slope from maximum number of line segments. Another idea was to find the mean of all the slopes from line segments. Next inorder to find a point, I picked a midpoint of a line segment whose slope is closest to mean value.\n",
    "\n",
    "Using these two values, I draw lanes on the original image.\n",
    "\n",
    "\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.2"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
