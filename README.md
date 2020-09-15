# UE4 Machine Learning: Image Segmentation
Documentation for "Machine Learning: Image Segmentation" Asset Pack for Unreal Engine 4

## Provided Blueprints
Descriptions of the most important Blueprints in the asset pack. Located in `[ProjectFolder]/Content/ImageSegmentation/Blueprints`
### Must have placed in your own level ###
- **BP_ObjectSegmentation:** Object Segmentation Blueprint is in charge of assigning unique segmentation colors by going through actor tags and assigning IDs and colors.
Provides automatic assignment of segmentation colors to the actors with tags and handles actor grouping.
    - Editable options:
      - `Segmentation`
         - `Colors Per Channel` - How many unique colors can be used in the segmentation mask (per color channel): must be a divisor of 255 (1, 3, 5, 15, 17, 51, 85, 255). Lower for debug purposes to view color diferences
         - `Actors To Classify` - Actors to include in segmentation (must have static mesh actor)

- **BP_SaveToFile:** Blueprint uses small Python script to export segmentation colors and actor tags to a .CSV file
    - Editable options:
      - `File Name` - Name of the segmentation .CSV file (default: Segmentation.csv)

- **BP_CameraOperator:** Camera Operator uses the cinematic camera with actor tag `EditorCam`, moves it randomly within the `Cam Spawn Area`, pointing it to the added target points, handles segmentation switching and screenshots.
    - Editable options:
      - `Delays`
         - `Screenshot Delay` - Delay for the console command
         - `Step Delay` - Delay between each step
         - `AAOn Delay` - Wait time for the Anti Aliasing to turn on and off
      - `Image Segmentation`
         - `Picture Amount` - Amount of screenshot-mask pairs
         - `Auto Start` - Start automatic screenshot taking process with the setings selected here. If unchecked: the level will start with player controlled pawn for debug purposes
         - `Resolution Width` - Screenshot width
         - `Resolution Height` - Screenshot height
         - `Switch Only Material Index` - If only 1 material on the object needs to be segmented set the index of the material here (first material is 0)
         - `Start Index` - Screenshots saved starting with this index

### Optional ###

- **BP_ObjectSpawner_Random:** Object Spawner that spawns all the actors from the list with random location limited by the `Spawn Volume` and random rotation and moves all of the objects down (Z axis) as much as possible to the "floor."
    - Editable options:
      - `Objects To Spawn` - Static Mesh Actors to include in the spawn and their amount

- **BP_ObjectSpawner_Show1:** Modified Object Spawner showing only 1 actor from the list in the center of the spawn area with default Z rotation. This can be used as an example of modifying the Object Spawner blueprint to your needs.
    - Editable options:
      - `Objects To Spawn` - Static Mesh Actors to include in the spawn and their amount
      - `Z Rotation` - Starting Z rotation of the actors
- **BP_SegmentationTestPlayer:** This is a test player pawn that can control the segmentation and screenshots while flying in the level, useful for debug purposes


## How to use the Blueprints in your own project
- How to use Automatic segmentation and object spawner:
  1. Make sure you have blueprints under "Must have placed in your own level" section in the level as well as a post process volume with Post Process Material `CustomToneMapper`, it also has to be disabled and set to infinite extent.
  2. Add Cinematic Camera and adjust the settings. This camera will be used to render. Set the actor tag on it to `EditorCam`
  3. Add Target Points (to be used as camera focus points)
  4. Add BP_ObjectSpawner_Random (found in Blueprints folder). Change the `Spawn Volume` Box Extent  to desired dimensions, this area will spawn actors
  5. Desired objects are to be added to `Objects to Spawn` array. They need to be of type `StaticMeshActor`. Steps to use a static mesh in this way:
     1. Right click in content browser and add Blueprint Class
     2. In search bar write static and select StaticMeshActor
     3. In the `StaticMeshComponent (Inherited)` set the static mesh, material, dimensions, etc.
     4. In `[Name](self)` set the Actor Tag and Mobility to Movable
     5. Add this Actor to the Objects to Spawn array and write the number of objects you want to spawn
   6. Click "Spawn Actors" button in BP_ObjectSpawner_Random. Test the motion with "Transform Actors". Actors need to be spawned to move them automatically. If the project was closed or rebuilt the link will be lost and it is necessary to remove actors manually and spawn new ones.
   7. Saved images, segmented image masks and actor tags are saved under: `[ProjectFolder]/Segmentation/` under `Images`, `Masks` and `Metadata` respectively.
 
- How to use Segmentation Test Player:
  1. Add BP_SegmentationTestPlayer and click Play to start (make sure in the BP_CameraOperator that the `Auto Start` checkbox is unchecked. Controls: Press "V" to view segmented image; Press "B" to capture a screenshot. This assumes you already have the required Blueprints placed in the level and have some static meshes with actor tags on them (otherwise there won't be anything to segment)
  2. Screenshots of the regular and the segmented images are saved in `[ProjectFolder]/Segmentation/Screenshots`.

## Provided Python Scripts
Scripts require OpenCV 2 and numpy libraries to run. They are meant to be run independently from the UE project but are set up for the project folder structure and require output from the automatic image capture. This is the final step to process the images from UE and create the JSON file in [COCO format](https://cocodataset.org/#home). They are located in `[ProjectFolder]/Content/ImageSegmentation/Scripts`. 
- **ue_segmentation:**
Supporting script but can be run independently. If run it shows the processed first image of the dataset where objects that are masked are shown with a bounding box and polygon points. This can be used to verify the segmentation is correctly done or if you don't need the COCO format file you can adapt this script to your needs.


- **ue_to_coco:**
Main script to run if you want the COCO format json file. You can adapt the JSON headers in the script to suit your needs. Output is by deafult to `[ProjectFolder]/Segmentation/Metadata/coco.json`
