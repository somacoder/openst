# Segmentation of staining image
Next, segment the nuclei in your images using Cellpose 2.2. We provide a model that we fine-tuned for segmentation of fresh-frozen, H&E-stained tissue, [here](https://github.com/danilexn/openst/blob/main/models/HE_cellpose_rajewsky). You can specify any other model that works best for your data - refer to the [cellpose](https://cellpose.readthedocs.io/en/latest/index.html) documentation.

```bash
openst segment --input=<path_to_input_image> --model=<path_to_model_or_name> --output=<path_to_output> --dilate=<how_much_to_dilate_the_mask> --diameter=20 --mask-tissue --metadata=<where_to_write_metadata>
```

!!! tip
     **If your samples contain very large cells** that cannot be segmented with the provided H&E model (e.g., adipocytes), you can perform a second round of segmentation with a cellpose model, adjusting the diameter parameter.

     ```bash
     openst segment --input=<path_to_input_image> --model=<path_to_model_or_name> --output=<path_to_output> --dilate=<how_much_to_dilate_the_mask> --diameter=50 --metadata=<where_to_write_metadata>
     ```

     Then, you can combine the segmentation masks of both diameter configurations.

     ```bash
     openst segment_merge --inputs=<path_to_input_images_as_list_more_than_one> --metadata=<where_to_write_metadata>
     ```

     Finally, you can create a report from the segmentation results

     ```bash
     openst report --metadata <path_to_metadata> --output=<path_to_html_file>
     ```

     This command will apply an "AND" between all images, to only preserve mask of non-overlapping, with the hierarchy provided in the files