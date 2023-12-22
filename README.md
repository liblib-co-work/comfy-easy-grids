# Easy Grids for ComfyUI

A set of custom nodes for creating image grids, sequences, and batches in [ComfyUI](https://github.com/comfyanonymous/ComfyUI).

## Current Features

* **Create Image Grid** - a node that lets you specify a grid size and then automatically queue up the full number of prompts required, outputting the current x and y position in the grid with each prompt. (It's actually a fully general `for` loop; you might find it useful for purposes other than making grids!)
* **Save Image Grid** - a modified Save Image node that accumulates images until reaching the number specified in its x_size and y_size inputs, then puts them into a grid. Supports labeling the grid through the 'annotations' input (though there are currently no nodes in this extension that can provide the right output - see Recommended Resources. )
* **Grid Floats** - Allows you to specify up to 6 floating-point numbers; the output will be selected using the 'index' input.
* **Grid Float List** - Similar to Grid Floats, but allows you to type in the list of floats manually - in case you need more than 6 or just find the selection widgets cumbersome. Commas, semicolons, colons, and whitespace are all valid delimiters - so e.g. `0.5,2.8:3.33;-2.5` is a valid input that should resolve to the list `[0.5, 2.8, 3.33, -2.5]`.
* **Float to Text** - Just converts a floating-point number to a text string. Useful for dynamically adjusting token or LoRA strengths.
* **Text Concatenator** - Transforms two strings of text into one string. Useful for adjusting your prompt according to the grid indexes.

## Future Features

* **More List-Selector Nodes** - Loop through integers! Strings! Checkpoints! LoRAs! Anything in ComfyUI that you have more than one of!
* **Annotations** - Labeling the output grid is currently supported...but not by any node that's part of this repository.
* **String Formatting** - Even adjusting a small number of token strengths with the current nodes quickly results in an absolute spaghetti of Text Concatenators. My goal is to have a node that provides `printf`- or `str.format`-like capability, substituting input strings at specified points in the text.
* **More visual feedback** - Progress bars and automatically updating counters on the Create and Save Image Grid nodes would be super helpful for knowing where you are in the loop. It might also make sense to have Save Image Grid update on the fly with the images it's received so far.

## How To Use

All nodes installed by this extension will be under "EasyGrids" in the Add Nodes menu.

1. Create a "Create Image Grid" node and a "Save Image Grid" node. 
2. Attach the x_size and y_size outputs of Create Image Grid to the matching inputs on Save Image Grid.[^1]  
3. Attach the x_index and/or y_index outputs of Create Image Grid to something you want to vary for each image on the grid. Many nodes in this extension are designed to generate lists which x_index or y_index can act as an index into, but you can also attach them to e.g. the seed input on the KSampler node or anything else that takes an integer as input.[^2] 
4. Set up your workflow as normal, attaching your final image output to Save Image Grid. (You may also want to attach it to a standard Save Image node if you want to save the individual images.)
5. Click the "Queue Full Grid" button on the Create Image Grid node and watch it go! (You can also mash the normal "Queue Prompt" button once for each image on the grid, but why would you do that?)

### Sample workflow

![Sample workflow using the index outputs to generate unique seeds](https://github.com/shockz0rz/comfy-easy-grids/blob/main/workflows/easygrids_workflow1.png?raw=true)

## How to Install

```
cd <ComfyUI repo>/custom_nodes
git clone https://github.com/shockz0rz/comfy-easy-grids.git
```

I don't believe there are any additional dependencies beyond what's already installed with ComfyUI; please correct me if I'm wrong on that!

## Recommended Resources

* [ComfyUI Custom Scripts](https://github.com/pythongosssss/ComfyUI-Custom-Scripts) - the Math Expression node is brilliant for turning your grid indexes into much more interesting numbers - like prompt token strengths, LoRA strengths, etc.
* [ComfyMath](https://github.com/evanspearman/ComfyMath) - also a good resource for turning your grid indexes into math.
* [ImagesGrid](https://github.com/LEv145/images-grid-comfy-plugin) - a similar idea implemented very differently. However, **the Grid Annotations node from ImagesGrid is fully compatible with the 'annotations' input on Easy Grids' Save Image Grid node**, and will likely be very helpful for labeling your grid until I finish my own implementation!


[^1]: The x_size and y_size inputs on Save Image Grid currently default to being selection widgets rather than input points. I'm working on fixing this, but for now, right-click on the Save Image Grid node and click "Convert x_size to input", then do the same with y_size.

[^2]: Keep in mind that the index outputs from Create Image Grid start from 1 rather than 0. The nodes in this repository are designed to expect that, but other nodes expecting list indexes may not.
