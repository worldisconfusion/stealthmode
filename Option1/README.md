# Cross-Camera Football Player Mapping

This project implements a system to map football players between two different camera views (broadcast and tacticam) of the same gameplay, assigning each player a consistent ID across both feeds. It utilizes YOLO for object detection and a Sports Re-Identification (ReID) model for feature-based matching.

## Setup and Environment Requirements

To set up the environment and run the code, you will need:

*   **Google Colab:** With access to a GPU (highly recommended for performance).
*   **Google Drive:** Mounted to your Colab environment to access input files.

**Dependencies:**

The necessary Python libraries and external repositories are:

*   **Python 3.11+**
*   **OpenCV (`opencv-python`)**: For video processing tasks like reading frames and writing output videos.
*   **NumPy**: For numerical operations, especially handling image data and feature embeddings.
*   **Ultralytics**: Provides the YOLO model and its functionalities.
*   **`gdown`**: Used for downloading files from Google Drive links.
*   **`scipy`**: Specifically for calculating cosine similarity between ReID embeddings.
*   **`torch` and `torchvision`**: Required for loading and using the Sports ReID model, as the `shallowlearn/sportsreid` repository is built on PyTorch.
*   **`tqdm`**: For displaying progress bars during video processing.
*   **`shallowlearn/sportsreid` repository**: This contains the specific Sports ReID model code used.

You can set up the environment and install the main dependencies by running the following commands in a code cell:

```bash

# Install gdown for downloading files from Google Drive
!pip install gdown --quiet

# Clone and setup sportsreid repository and install its requirements
!git clone https://github.com/shallowlearn/sportsreid.git sportsreid
%cd sportsreid
!pip install -r requirements.txt
%cd .. # Navigate back to the main directory

```

## Input Files

You will need the following input files, preferably stored in your Google Drive and accessible via shareable links or by updating the code to load them from a different path:

*   **Broadcast Video:** `broadcast.mp4` - Video from the broadcast camera angle.
*   **Tacticam Video:** `tacticam.mp4` - Video from the tacticam angle.
*   **Object Detection Model:** `yolov11_players.pt` (or the name you use) - The fine-tuned YOLOv11 model for player detection.

The code is currently configured to download these using specific Google Drive file IDs. **You must replace the placeholder IDs in the setup cell with your actual file IDs** or modify the code to load the files from your preferred location after mounting Google Drive.

## Running the Code

1.  **Mount Google Drive:** Ensure your Google Drive is mounted in the Colab environment. This is done in the first code cell.
2.  **Setup Environment and Download Data:** Run the cell that installs `gdown`, clones and sets up the `sportsreid` repository, and downloads the video and YOLO model files using their Google Drive IDs. **Remember to update the file IDs before running.**
3.  **Load Models:** Run the cell to load the YOLO object detection model and the Sports ReID feature extractor.
4.  **Process Videos and Extract Features:** Execute the cell that processes both video files frame by frame, performs YOLO detection, crops player bounding boxes, and extracts ReID feature embeddings for each detection.
5.  **Develop Cross-Camera Matching Strategy:** Run the cell that implements the logic to compare detections between the two videos and find potential matches based on ReID similarity.
6.  **Assign Consistent IDs:** Execute the cell that uses the matching results to assign consistent player IDs across detections from both video streams.
7.  **Visualize Mapping:** Run the cell that generates the side-by-side visualization video (`cross_camera_mapping_vis.mp4`) showing the assigned consistent IDs on both video feeds.
8.  **View Output:** The visualization video will be saved in the `/content/` directory by default. You can download it from the Colab file browser to view the results of the cross-camera mapping.

## Output

The primary output of this project is the visualization video:

*   `cross_camera_mapping_vis.mp4`: A video showing the broadcast and tacticam feeds side-by-side with detected players annotated with consistent player IDs assigned through the cross-camera mapping process.