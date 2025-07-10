# Football Player Tracking with Re-Identification

This project implements a football player tracking system with enhanced re-identification capabilities using YOLO for detection and a custom association logic incorporating a sports Re-Identification (ReID) model.

## Setup and Environment Requirements

To set up the environment and run the code, you need to have Google Colab with access to a GPU (recommended for faster processing) and Google Drive mounted to download necessary files.

The main dependencies are:

*   **Python 3.11+**
*   **OpenCV (`opencv-python`)**: For video processing.
*   **NumPy**: For numerical operations.
*   **Ultralytics**: For the YOLO model and its tracking functionality.
*   **`gdown`**: To download files from Google Drive.
*   **`scipy`**: For calculating cosine similarity for ReID embeddings.
*   **`torch` and `torchvision`**: For loading and using the Sports ReID model.
*   **`shallowlearn/sportsreid` repository**: This repository contains the ReID model code.

You can install the primary Python dependencies using pip:

```bash
!git clone https://github.com/shallowlearn/sportsreid.git sportsreid
%cd sportsreid
!pip install -r requirements.txt
%cd .. # Navigate back to the main directory

```

## Running the Code

1.  **Upload Input Files:** Ensure your input video (`video.mp4` or the name you are using) and the YOLO model file (`best.pt` or the name you are using) are accessible in your Colab environment. The current code is set up to download them from specific Google Drive links, so make sure those links are correct or update the code to load from a different location if needed.
2.  **Execute Cells:** Run the notebook cells sequentially. The code will perform the following steps:
    *   Install dependencies and clone the sports ReID repository.
    *   Load the video frames and the YOLO model.
    *   Setup video writers for both the original tracking output and the ReID-enhanced tracking output.
    *   Implement and run the main tracking loop with ReID feature extraction and enhanced association logic.
    *   Save the two output videos (`tracked_video.mp4` and `tracked_video_reid.mp4`).
3.  **View Output:** The output videos will be saved in the `/content/` directory (or your specified output path). You can download them from the Colab file browser or mount your Google Drive and specify a Drive path for saving.

## Input Files

*   **Input Video:** `video.mp4` (or your video filename). The code currently downloads this from a Google Drive link.
*   **Object Detection Model:** `best.pt` (or your model filename). The code currently downloads this from a Google Drive link.
*   **Sports ReID Model:** The code utilizes the model from the cloned `shallowlearn/sportsreid` repository.
