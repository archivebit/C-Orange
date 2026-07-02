# C-Orange: A Real-World Benchmark Dataset for Object Detection and Counting of Oranges in Orchard Environments

A real-world orange counting benchmark jointly modeling occlusion, fruit clustering, illumination variability, and scale variation.

## Structure

```
Dataset/
    Full Dataset/
    Occlusion/
    Fruit Cluster/
    Illumination Variability/
    Scale Variation/
        Raw Image/                # raw .jpg
        YOLO_BOX_Algorithmic/     # YOLO bbox labels (seg-to-box, algorithmic)
        YOLO_BOX_Human/           # YOLO bbox labels (annotated through Roboflow)
        YOLO_SEG/                 # YOLO segmentation polygon labels
        COCO/                     # COCO-format JSON + images
Source code/
    YOLOv5.ipynb
    YOLOv8.ipynb
    SSD300.ipynb
    FasterRCNN.ipynb
    MaskRCNN.ipynb
README.md
```

Each challenging-condition partition contains the complete set of images,
split 80/10/10 across train/valid/test.

## Annotation formats

* **YOLO_BOX_Algorithmic/labels/**: one `.txt` per image, lines of
  `class cx cy w h` (normalized to [0, 1], class 0 = orange). Bounding
  boxes were algorithmically derived from the segmentation polygons by
  computing the minimum and maximum x and y coordinates across all
  polygon vertices, defining the smallest enclosing rectangle.
* **YOLO_BOX_Human/labels/**: same format. These bounding boxes were
  annotated through Roboflow.
* **YOLO_SEG/labels/**: one `.txt` per image, lines of
  `class x1 y1 x2 y2 ... xN yN` (polygon points, normalized).
* **COCO/_annotations.coco.json**: standard COCO with `bbox` and
  `segmentation` fields.

## Using the dataset

Each partition folder is self-contained and can be loaded independently. Pick
the annotation format that matches your model:

| Model               | Annotation folder              | Loaded as                |
|---------------------|--------------------------------|--------------------------|
| YOLOv5, YOLOv8, SSD | `YOLO_BOX_Algorithmic/` (or `YOLO_BOX_Human/`) | `data.yaml` + `images/`, `labels/` |
| Faster R-CNN, Mask R-CNN | `COCO/`                   | `_annotations.coco.json` + images |

The `data.yaml` inside each YOLO folder uses `train/images`, `valid/images`,
`test/images` as the standard YOLO layout. The COCO folder uses a flat
`train/`, `valid/`, `test/` structure with one `_annotations.coco.json`
per split.

## Source code

One `.ipynb` per model architecture used in the benchmark evaluation,
containing the training and evaluation pipeline used for that model. All notebooks are designed to run in **Google Colab** with Google Drive mounted, and expect the dataset at 
`/content/drive/MyDrive/Dataset/{EXP1_ALL, EXP2_OCCLUSION, EXP3_CLUSTER, EXP4_ILLUMINATION, EXP5_SCALE}/{YOLO_BOX, COCO}/` — rename the shipped folders (Full Dataset, Occlusion, Fruit Cluster, Illumination Variability, Scale Variation) to the EXP* names and pick either `YOLO_BOX_Algorithmic/` or `YOLO_BOX_Human/` as `YOLO_BOX/` before running.