<div align="center">
    <h1>Frequently Asked Questions</h1>
</div>




\#129  
**Q: Why do some input arrays contain only zeros in the CellMap Segmentation Challenge dataset?**

**A:** Yes, this is expected. Many crops do not contain all organelles, resulting in input arrays filled with zeros or `False` values. This occurs because not every crop includes every organelle type.

---

**Q: Is there a way to crop only the image regions with ground truth labels and ignore the zero-only regions?**

**A:** If you use the **included data loading utilities**, only regions with data should be loaded. Otherwise, you can use the [`training_crop_manifest`](https://github.com/user-attachments/files/18716486/train_crop_manifest.csv) to export cropped versions of the raw data that exclude empty regions.

*Note: This behavior is normal and does not indicate an issue with your data or processing pipeline.*

---


\#130  
**Q: How can I access the ground truth data for the CellMap Segmentation Challenge?**

**A:** The ground truth data for the CellMap Segmentation Challenge is available in the `training` folder within the dataset. Each organelle has a corresponding subfolder containing the ground truth labels. For example, for mitochondria, you can find the ground truth data in the `training/mitochondria` directory. Ensure you refer to the specific organelle's subfolder to access the correct ground truth labels.​

*Note: Accurately locating and utilizing the ground truth data is crucial for effective model training and evaluation in the challenge.*

---


\#108  
**Q: Is there a limit to the number of submissions I can make per day in the CellMap Segmentation Challenge?**

**A:** Currently, there is no daily submission limit for participants. However, the organizers may implement a daily or lifetime limit if the overall submission volume becomes too high or if there is suspicion of misuse of the scoring system to enhance entries.

---


**Q: How long does it take to receive evaluation scores after submitting a prediction result?**

**A:** Evaluations can take up to 3 hours to complete. The duration primarily depends on the number of unique objects within the volumes being evaluated as instance segmentations.

*Note: If you encounter any challenges or have further questions, feel free to ask for more in-depth assistance to troubleshoot your entry.*

---


\#105

**Q: How can I define training hyperparameters in the CellMap Segmentation Challenge?**

**A:** The training pipeline accepts a `config_path` argument that points to a Python configuration file. This file allows you to set various hyperparameters and configurations for model training. Key parameters include:​

* **`model_save_path`**: Specifies where to save model checkpoints. Default is `checkpoints/{model_name}_{epoch}.pth`.​

* **`logs_save_path`**: Determines where to save logs for TensorBoard. Default is `tensorboard/{model_name}`. You can monitor training progress by running `tensorboard --logdir <logs_save_path>` in the terminal.​

* **`datasplit_path`**: Indicates the path to the datasplit file that defines the train/validation split for the dataloader. Default is `datasplit.csv`.​

* **`validation_prob`**: Sets the proportion of the dataset to use for validation, used if the specified datasplit CSV does not already exist. Default is `0.15`.​

* **`learning_rate`**: Defines the learning rate for the optimizer. Default is `0.0001`.​

* **`batch_size`**: Sets the batch size for the dataloader. Default is `8`.​

* **`input_array_info`**: A dictionary containing the shape and scale of the input data. Default is `{'shape': (1, 128, 128, 128), 'scale': (1.0, 1.0, 1.0)}`.​

By customizing these parameters in your configuration file, you can tailor the training process to your specific requirements.

---


\#100  
**Q: Why do different data chunks (e.g., `jrc_mus-kidney-3`, `jrc_mus-liver-zon-1`) have varying numbers of categories in their `groundtruth` folders?**

**A:** Each dataset focuses on specific organelles or structures, so the number of categories in the `groundtruth` folders can vary accordingly.​

---

**Q: In the `all` groundtruth files, does each unique index correspond to the same category across different datasets?**

**A:** Yes, in the `all` groundtruth files, each unique index consistently represents the same category across different datasets.​

---

**Q: Is the downloaded data intended for semantic segmentation, and how can I obtain labels for instance segmentation?**

**A:** The provided groundtruth data is primarily for semantic segmentation. To derive instance segmentation labels, you can apply connected component labeling or watershed algorithms to the semantic segmentation masks.​

---

**Q: I encountered some crops, like `jrc_hela-3/crop60`, where the groundtruth is all zeros, and the original image appears as random noise. Is this expected?**

**A:** Yes, this is expected. Some crops may not contain any annotated structures, resulting in groundtruth files filled with zeros. Additionally, certain regions might appear as noise due to the imaging process or the specific area captured.​

*Note: For more detailed information, you can refer to the [discussion thread](https://github.com/janelia-cellmap/cellmap-segmentation-challenge/discussions/100) on this topic.*

---


\#101

**Q: Why do the ground truth annotations in certain crops contain category IDs that differ from the required prediction categories?**

**A:** The discrepancy arises because the provided ground truth annotations include a broader set of category IDs, while the challenge focuses on a specific subset for prediction. Participants should map the ground truth categories to the required prediction categories as specified in the challenge guidelines.​

---

**Q: What does the category ID '0' represent in the ground truth annotations?**

**A:** In the ground truth annotations, the category ID '0' represents the background or regions without any labeled structures. Participants should ensure that their models distinguish between background (ID '0') and the relevant [organelle categories](https://janelia-cellmap.github.io/cellmap-segmentation-challenge/annotation_classes.html#detailed-class-descriptions).​

---

**Q: If I encounter class '2' in the ground truth, should I map it to '60' or keep it as '2'?**

**A:** Participants should refer to the challenge's category mapping guidelines to determine the correct mapping for each class. It's essential to follow the provided mappings to ensure consistency in predictions.​

*Note: For detailed discussions and clarifications on annotation ID issues, please refer to [this discussion thread](https://github.com/janelia-cellmap/cellmap-segmentation-challenge/discussions/101).*

---


\#94  
**Q: Where can I find the `train_crop_manifest.csv` file in the CellMap Segmentation Challenge dataset?**

**A:** The `train_crop_manifest.csv` file, which provides details about the labeled training crops and their corresponding raw FIB-SEM images, is now available. You can access it directly from the challenge repository:​

* **Download Link:** [train\_crop\_manifest.csv​](https://github.com/user-attachments/files/18716486/train_crop_manifest.csv)

This manifest includes essential information such as `voxel_size`, `translation`, and `shape` for each labeled crop, aiding in accurate data alignment and analysis.​

*Note: For further details and updates, refer to the [discussion thread](https://github.com/janelia-cellmap/cellmap-segmentation-challenge/discussions/94) in the challenge repository.*

---


\#93  
**Q: Is the highest resolution level (s0) consistently 2nm across all datasets in the CellMap Segmentation Challenge?**

**A:** No, the highest resolution level (s0) varies between datasets for electron microscopy (EM) and between crops for ground truth.

*Note: It's essential to verify the specific resolution details for each dataset or crop when working on the challenge to ensure accurate analysis and model training.*

---


\#128

**Q: Is it normal to encounter missing chunks in the datasets after downloading them using the default `csc fetch-data` command?**

**A:** Yes, this is expected behavior. By default, the `csc fetch-data` command downloads only the raw data blocks that have corresponding ground truth annotations. This approach optimizes storage by fetching only the annotated regions. Consequently, when viewing datasets like `jrc_cos7-1a` at the s2 level in Fiji, you might notice missing chunks or gaps in areas without annotations.

---


**Q: How can I download additional raw data beyond the annotated regions?**

**A:** To fetch more raw data surrounding the annotated blocks, you can use the `--raw-padding` parameter with the `csc fetch-data` command. This parameter allows you to specify additional padding around the annotated regions. For example, to add a padding of 128 voxels, execute:​

`csc fetch-data --raw-padding 128`

Be cautious when increasing the padding size, as it may result in downloading the entire raw dataset, which could require substantial storage space.

---


**Q: Where can I find information about the annotated blocks for each dataset?**

**A:** You can refer to the [annotated blocks information](https://janelia-cellmap.github.io/cellmap-segmentation-challenge/available_data.html) for details on the specific regions that have been annotated in each dataset. This resource will help you understand which parts of the data have corresponding ground truth annotations.

*Note: Adjusting the `--raw-padding` parameter allows for flexible data retrieval based on your storage capacity and analysis needs.*

---


\#82

**Q: What should I do if I encounter a "bad\_malloc" error while running `train_2D.py`?**

**A:** The "bad\_malloc" error typically indicates that the system has run out of GPU memory during the training process. To address this issue, consider the following steps:

1. **Reduce the Batch Size:** Lowering the batch size decreases the memory required for each training iteration. You can adjust the batch size in your training script or configuration file.

2. **Use Smaller Input Sizes:** Reducing the dimensions of the input images can significantly lessen memory consumption. Ensure that the smaller input size is still suitable for your model's architecture and the problem domain.

3. **Optimize Model Architecture:** Simplifying the model by reducing the number of layers or parameters can help in lowering the memory footprint.

4. **Upgrade GPU Hardware:** If possible, consider using a GPU with more memory to accommodate larger models or batch sizes.

*Note: These suggestions are general guidelines to mitigate memory allocation errors during training.*

---


\#112

**Q: Why do I receive an "AssertionError: No valid training datasets found" when setting `classes = ["ribo"]` in the training script?**

**A:** The 'ribo' class is included as a potential category; however, in the current dataset, there are no annotations for ribosomes. Consequently, datasets for the 'ribo' class are empty, containing only background. This absence leads to the "No valid training datasets found" assertion error during training.

*Note: For more details, refer to the [discussion thread](https://github.com/janelia-cellmap/cellmap-segmentation-challenge/issues/112) in the challenge repository.* 

---


\#113

**Q: Why are classes 56-58 missing from the annotation classes in the CellMap Segmentation Challenge?**

**A:** Classes 56-58 correspond to components of insulin secretory granules and are not included in the challenge's annotation classes. Specifically:

* **Class 56:** Insulin secretory granule membrane (`isg_mem`)  
* **Class 57:** Insulin secretory granule lumen (`isg_lum`)  
* **Class 58:** Insulin secretory granule insulin (`isg_ins`)

These classes are excluded because they are not part of the challenge's focus.

*Note: For a comprehensive [list of annotation classes and their descriptions](https://janelia-cellmap.github.io/cellmap-segmentation-challenge/annotation_classes.html#detailed-class-descriptions), please refer to the project's documentation.*

---


Email

**Q: Do different organelle labels have different resolutions?**

**A:** Yes, organelle labels can appear at different resolutions, but the exact resolution varies. Nuclei may be labeled at 8nm or 32nm, while other organelles, like mitochondria, may have higher-resolution annotations. Larger organelles are more commonly labeled at lower resolutions, while smaller organelles tend to have higher-resolution labels.

*Note: More details on resolution levels can be found in [this discussion](https://github.com/janelia-cellmap/cellmap-segmentation-challenge/discussions/93).*

---

**Q: When fetching data without `--fetch-all-em-resolutions`, does each crop contain different resolution data based on the labels present?**

**A:** Yes, the EM data resolution varies depending on the organelles in each crop. If a crop contains only nuclei, it may have only 32nm EM data, while a crop with mitochondria may have 8nm EM data. This ensures that the data aligns with the labeled structures in each crop.

---

**Q: If a crop contains 8nm resolution EM data, does it automatically include lower resolutions like 16nm or 32nm?**

**A:** Yes, lower resolutions are always included in the dataset. If 8nm resolution EM data is available, the 16nm and 32nm versions can be derived from it. Lower-resolution EM data is always part of the download.

*Note: See [this discussion](https://github.com/janelia-cellmap/cellmap-segmentation-challenge/discussions/100) for more details on dataset structure.*

---

**Q: Does the dataloader automatically exclude nuclei samples with 32nm resolution?**

**A:** No, the dataloader does not automatically exclude nuclei samples at 32nm resolution. Filtering must be manually configured in the datasplit settings.

---

**Q: Is there a way to filter out empty data when generating a datasplit?**

**A:** Yes, the latest update (PR \#118) introduced filtering to exclude empty data when creating a datasplit. Running the command:

`csc make-datasplit -c mito,nuc -s 6.0 6.0 6.0`

ensures that the generated `datasplit.csv` only includes FIB-SEM and ground truth data with valid annotations.

*Note: Full details in [this discussion](https://github.com/janelia-cellmap/cellmap-segmentation-challenge/discussions/128).*

---

**Q: Can the dataset include the mean and standard deviation (std) for normalization?**

**A:** Currently, the mean and standard deviation of the FIB-SEM dataset are not reported, but the team may consider adding this information in the future.


<!-- trigger rebuild -->


