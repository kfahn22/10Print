# Demo of how to push a dataset of 10Print images to the Hugging Face hub

This repo demos how how to create a dataset of labeled 10Print images and push them to the Hugging Face hub. You can find the dataset [here](https://huggingface.co/datasets/kfahn/10Print).

The 10Print algorithm is an easy way to make random-looking mazes. For each square in a grid, either an upward or downward slash is drawn based on some random probability. If you would like to learn more about the 10Print algorithm, I recommend that you watch Daniel Shiffman's [10Print Coding Challenge](https://thecodingtrain.com/challenges/76-10Print).

![image](assets/10.png)

- Install the dependencies

`!pip install huggingface_hub datasets`

- Log into the Hugging Face hug with your WRITE access token

`from huggingface_hub import notebook_login`  
`notebook_login()`

- Create a folder for the images

`import os`
`generated_images_dir = "/content/images"`
`os.makedirs(generated_images_dir, exist_ok=True)`

- I created a function to define a custom colormap

`import random`
`import matplotlib.colors as mcolors`

`import matplotlib.pyplot as plt`
`import numpy as np`

// import matplotlib as mpl
`from matplotlib.colors import LinearSegmentedColormap`
`colors1 = mcolors.CSS4_COLORS`
`colors2 = mcolors.XKCD_COLORS`

`def make_colormap(n_bins, color_choices=None):`
"""Return a LinearSegmentedColormap
color1 and color2 are the two colors that are interpolated between
n_bins: Discretizes the interpolation into bins
"""
`if color_choices is None:`
`colors = colors2`
`color_name1 = random.choice(list(colors.keys()))`
`color_name2 = random.choice(list(colors.keys()))`
`color1 = colors[color_name1]`
`color2 = colors[color_name2]`
`cmap_name = color_name1 + '/' + color_name2`
`color_choices = [color1, color2]`
`for n_bin in range(n_bins):`
`cmap = LinearSegmentedColormap.from_list(cmap_name, color_choices, N=n_bin)`
`return cmap, color_name1, color_name2`

- I also created some custom markers

`from matplotlib.path import Path`

// Define (x, y) vertices

`v3 = [ (-1, -0.66),`
`(-1, -1),`
`(-0.66, -1),`
`(1, 0.66),`
`(1, 1),`
`(0.66, 1)`
`]`

`v4 = [ (-1, 0.66),`
`(-1, 1),`
`(-0.66, 1),`
`(1, -0.66),`
`(1, -1),`
`(0.66,-1)`
`]`

This is the line from the jsonl file about the image:
`{"file_name": "10.png", "text": "a sand yellow 10Print pattern on a prussian blue background"}`
