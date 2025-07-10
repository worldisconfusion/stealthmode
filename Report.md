# Project Report: Football Player Tracking with Re-Identification

Hey there,

This project was all about tracking football players in a video and trying to make sure we could keep track of who's who, even if they disappeared for a bit and came back.

## 1. My Approach

My main goal was to identify each player and keep their ID consistent throughout the video. I decided to use a few different pieces to make this happen:

*   **Finding Players:** I used a pre-trained YOLOv11 model that was fine-tuned for detecting players, goalkeepers, and referees. It's pretty good at spotting everyone in each frame.
*   **Keeping Track Initially:** For the initial tracking, I relied on the built-in tracking feature of the Ultralytics YOLO library, which uses something called ByteTrack. It's good at assigning temporary IDs and following players from one frame to the next based on where they move and how they look.
*   **Improving Re-identification:** This was the trickier part. To handle players leaving and coming back into the frame, I integrated a specific sports Re-Identification (ReID) model from the `shallowlearn/sportsreid` project. The idea here is that this model can generate a unique 'fingerprint' (a feature embedding) for each player based on their appearance. I then used these fingerprints in my own logic to try and match players who reappeared with their old IDs. My custom logic basically looked at both how much the bounding boxes overlapped (like ByteTrack does) and how similar their ReID fingerprints were to decide if it was the same player.

## 2. What I Tried and How It Went

Most of my effort went into getting the ReID part working and integrated smoothly.

*   Getting the `shallowlearn/sportsreid` repository set up took a bit of figuring out, finding where the model loading code was and how to use it.
*   Building the function to extract the ReID features from cropped images was a key step. I had to make sure the image was the right size and format for the ReID model.
*   The biggest challenge was definitely building the custom association logic. Deciding how to combine the standard tracking cues (like overlap) with the ReID similarity was tricky. I started with a simple combination, and I think there's room to play with the weights or use a more advanced matching method if I had more time. I focused mainly on matching players since that was the main goal.

## 3. Hiccups Along the Way

Ran into a few issues, as expected:

*   Setting up the environment and making sure all the libraries were installed correctly took a little bit of troubleshooting.
*   Initially, I had some trouble extracting the correct bounding box information from the YOLO tracking results because I was using the wrong attribute (`.ltrb` instead of `.xyxy`). Figuring that out from the error messages took a moment.
*   Getting the Google Drive mounting and file downloads to work reliably sometimes required double-checking paths.

## 4. What's Still Missing and Next Steps

While I got the core tracking with ReID feature extraction and a custom association logic working, there are definitely areas for improvement:

*   **Fine-tuning Association:** The parameters for combining IoU and ReID similarity in the association logic could be tuned more rigorously to optimize performance on the video.
*   **Quantitative Evaluation:** I did a visual comparison of the videos, which was helpful, but a more formal quantitative evaluation using metrics like MOTA or IDF1 would give a clearer picture of the accuracy improvement. This would require ground truth data, which I didn't have.
*   **Handling Complex Scenarios:** The current logic might struggle with full occlusions or when many players are very close together. More advanced techniques might be needed there.
*   **Optimization:** The current setup processes the video offline. For real-time applications, I'd need to look into optimizing the runtime efficiency, maybe by using a more lightweight ReID model or optimizing the feature extraction process.

If I had more time, I would definitely focus on refining the association logic, trying different ways to combine the ReID features with motion data, and setting up a more systematic way to evaluate the performance.

---

Hopefully, this report clearly explains what I did and how it went!
