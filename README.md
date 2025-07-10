# Stealth Mode: Player Re-Identification & Cross-Camera Mapping Submission

Hi there,

This repository contains my submission for the Stealth Mode assignment. I've tackled both the single-feed re-identification and the cross-camera player mapping challenges as part of solving this assignment.

## Repository Contents

*   **Code:** Source code for both the single-feed re-identification and cross-camera mapping tasks. (Structure this clearly in your repo, e.g., separate folders or notebooks).
*   **README.md:** This file provides setup and running instructions.
*   **Report:** A brief report detailing the approach, techniques, challenges, and future work for both tasks.
*   **Output Videos:** Resulting videos from running the implemented solutions.

## Setup and Environment

To run the code, you will need a Python environment (preferably in Google Colab with GPU access) and the following main dependencies:

*   **OpenCV**
*   **NumPy**
*   **Ultralytics**
*   **`gdown`**
*   **`scipy`** (Used in Cross-Camera Mapping)
*   **`torch` and `torchvision`** (Required for the Sports ReID model)
*   **`tqdm`** (For progress bars)
*   **`shallowlearn/sportsreid` repository**: This needs to be cloned and its requirements installed.

You can typically install dependencies using pip. Ensure you clone the `sportsreid` repository and install its specific requirements as well.

## How to Run

1.  **Setup Environment:** Install the necessary Python libraries and clone the `shallowlearn/sportsreid` repository.
2.  **Obtain Input Files:** Make sure you have the required video files (`broadcast.mp4`, `tacticam.mp4`, `15sec_input_720p.mp4`) and the YOLO model (`yolov11_players.pt` or `best.pt`) accessible, likely by downloading them from the provided Google Drive links or having them in a mounted Google Drive.
3.  **Execute Code:** Run the relevant scripts or notebook cells for the task you want to execute (either the single-feed re-identification or the cross-camera mapping). Follow the specific instructions within the code or accompanying documentation for each task regarding file paths and parameters.
4.  **View Output:** The output videos and any other result files will be saved to a specified location (e.g., `/content/` or a Google Drive path).

Refer to the detailed report for more in-depth explanations of the approaches and implementation specifics for each task.