# Project Report: Cross-Camera Football Player Mapping

Alright, here's a breakdown of my project on mapping football players across two cameras, structured with the sections you asked for.

## Your approach and methodology.

My main goal here was to take two videos of the same game, shot from different cameras (a broadcast view and a tactical view), and figure out which player in one video was the same player in the other video, giving them a consistent ID across both.

My approach involved several steps:

First off, I needed to **find the players** in both videos. For this, I used the provided YOLOv11 model that was already trained to spot players, goalkeepers, and referees. I ran this model on every single frame of both the broadcast and tactical videos.

Once I had the players detected, I needed a way to **tell players apart based on how they look**. This is where the Re-Identification (ReID) part came in. I used a Sports ReID model from that `shallowlearn/sportsreid` repository. For each player detected by YOLO, I cropped out their image and ran it through the ReID model to get a unique feature vector, kind of like an appearance fingerprint.

Then came the core challenge: **matching players between the two cameras**. I decided to try matching players who appeared in the same frame in both videos. My strategy was to compare the ReID feature embeddings of every detected player in the broadcast frame with every detected player in the tactical frame. If the similarity between their embeddings was high enough (above a certain threshold I set), I considered them a potential match.

Finally, based on these matches, I needed to **assign consistent IDs**. My current approach to ID assignment is based on the frame-by-frame matches. When I found a match between a broadcast detection and a tactical detection in the same frame, I assigned them a common player ID. If either of those detections had already been matched in a previous frame and assigned an ID, I tried to propagate that existing ID. If neither had an ID, I assigned a new one.

To **see how well it worked**, I generated a visualization video showing both camera feeds side-by-side with the assigned player IDs drawn on the bounding boxes. This let me visually check if the players were being matched correctly across the views.

## Techniques you tried and their outcomes.

The main techniques I used were:

*   **YOLOv11 for Detection:** This worked pretty well for finding the players in both videos. The model was already fine-tuned, so I didn't need to do much here other than run inference.
*   **Sports ReID Model for Feature Extraction:** Using the `FeatureExtractor` from the `shallowlearn/sportsreid` repo allowed me to get appearance embeddings. I had to make sure the input images were correctly cropped and preprocessed for the model.
*   **Cosine Similarity for ReID Comparison:** I used cosine similarity to compare the ReID embeddings. This is a standard way to measure how 'alike' two feature vectors are. I experimented a bit with the similarity threshold – setting it too low resulted in too many incorrect matches, while setting it too high meant missing some correct matches. The value of 0.5 seemed like a reasonable starting point, but it definitely impacts the results.
*   **Simple Frame-by-Frame Matching based on ReID:** My initial matching logic was to just compare players in the same frame. The outcome was that it could find matches for players clearly visible in both views at the exact same time. However, it didn't handle players who might appear in one view slightly before or after the other, or players who were only visible in one view.
*   **Basic ID Assignment:** The outcome of my current ID assignment is that it links players *at the moment of match* in a specific frame. It assigns a new ID for each initial match found. Propagating IDs across frames and linking matches over time is more complex and an area for future work.

## Challenges encountered.

Building this pipeline definitely had its challenges:

*   **Setting up the Environment:** Getting the `sportsreid` repository cloned and its specific dependencies installed correctly alongside the other libraries in Colab required careful steps.
*   **Integrating the ReID Model:** While the `FeatureExtractor` simplified loading, understanding how to feed it the cropped images correctly and handling potential issues during feature extraction for small or unclear detections was something I had to work through.
*   **Designing the Matching Logic:** Coming up with a robust way to match players across two videos was probably the hardest part. Just comparing frames by index and relying solely on ReID similarity feels a bit basic, and I know it's not ideal for real-world scenarios with potential time shifts or partial visibility.
*   **Consistent ID Assignment:** Making sure the IDs were truly consistent over the entire duration of both videos, especially for players who might be matched in multiple frames or reappear after being absent, was challenging with the simple frame-by-frame matching approach.

## If incomplete, describe what remains and how you would proceed with more time/resources.

The project provides a foundation, but it's definitely incomplete in terms of achieving highly accurate and robust cross-camera re-identification throughout the entire video.

What remains is primarily to **significantly improve the cross-camera matching and ID assignment logic**.

With more time and resources, I would proceed by:

*   **Implementing more sophisticated temporal handling:** Instead of just comparing frame `N` of broadcast with frame `N` of tacticam, I'd look at detections within a small time window around frame `N` in both videos to account for potential synchronization differences.
*   **Developing a more advanced matching algorithm:** I'd explore techniques beyond simple pairwise comparison, maybe something like graph matching where I build a graph of potential matches over a temporal window and find the best overall set of matches.
*   **Integrating tracking within each view first:** A better approach would be to first track players individually within the broadcast video and separately within the tacticam video. Then, I would focus on matching these *tracks* across the two videos using their average ReID embeddings, spatial location (if possible), and temporal overlap. This would lead to more stable and consistent global IDs.
*   **Refining ID Propagation:** I'd need a more robust system to manage global player IDs, linking new matches or re-appearing players back to existing tracks and IDs.
*   **Quantitative Evaluation:** Ideally, I'd find or create some ground truth data (manual annotations of which player is which in both videos over time) to quantitatively measure the accuracy of the mapping using standard metrics.

This project was a good starting point, and the steps I've outlined would be crucial for building a truly reliable cross-camera player mapping system.

---

Hope this version fits what you were looking for! Let me know if you want to adjust anything.