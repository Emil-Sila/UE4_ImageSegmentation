# UE4 Machine Learning: Image Segmentation
Documentation for "Machine Learning: Image Segmentation" Asset Pack for Unreal Engine 4

## Provided Blueprints
Descriptions of the most important Blueprints in the asset pack. Located in `[ProjectFolder]/Content/ImageSegmentation/Blueprints`
### Must have placed in your own level ###
- **BP_ObjectSegmentation:**
Provides automatic assignment of segmentation colors to the actors with tags and handles actor grouping.
    - Editable options:
      - `test` - 

- **BP_SaveToFile:**
    - Editable options:
      - `test` - 

- **BP_CameraOperator:**
    - Editable options:
      - `test` - 

### Optional ###

- **BP_ObjectSpawner_Random:**
    - Editable options:
      - `test` - 

- **BP_ObjectSpawner_Show1:**
    - Editable options:
      - `test` - 

- **BP_SegmentationTestPlayer:**
    - Editable options:
      - `test` - 

## How to use the Blueprints in your own project
1.
2.
3.

## Provided Python Scripts
Scripts require OpenCV 2 and numpy libraries to run. They are meant to be run independently from the UE project but are set up for the project folder structure and require output from the automatic image capture. This is the final step to process the images from UE and create the JSON file in [COCO format](https://cocodataset.org/#home). They are located in `[ProjectFolder]/Content/ImageSegmentation/Scripts`
- **ue_segmentation:**
Supporting script but can be run independently. If run it shows the processed first image of the dataset where objects that are masked are shown with a bounding box and polygon points. This can be used to verify the segmentation is correctly done or if you don't need the COCO format file you can adapt this script to your needs.


- **ue_to_coco:**
Main script to run if you want the COCO format json file. You can adapt the JSON headers in the script to suit your needs. Output is by deafult to `[ProjectFolder]/Segmentation/Metadata/coco.json`
