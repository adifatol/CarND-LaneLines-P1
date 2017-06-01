
#Finding Lane Lines on the Road

---

## Implementation

### lane_find_pipeline()
The first step was to add a function called lane_find_pipeline() in order to be used in the existing process_image(), which is used to process  each frame in the videos. This function includes all the steps defined in the "Concepts" part. 

***Flow***
--
In the beginning some useful parameters were declared which were later updated. After this declaration, the image/frame is converted to grayscale and gaussian blur is applied in order for the image to be prepared for canny edge detector. This function then outputs an image containing only the edges in the original frame. This is applied because the lane lines are usually white/yellow lines over black/gray asphalts and the edge of the lines should be very sudden. On the image containing these edges, the hough_lines function is used in order to find the lanes, but not before defining a region of interest. The region of interest assures that the lines are not polluted with points outside the actual lanes. Some offsets are dynamically calculated based on the image shape in order to have the minimal interest area.

This pipeline was then tested on the test_images folder and output images were saved to test_images_output. The parameters were tweaked in order to get better results.

### draw_lines()
Second step was to improve the draw_lines() function in order to draw a continuous line for left and right lanes. First the start & end points for the segments detected through hough_lines function were separated on left/right based on the slope calculated by polyfit function. After that, new start & end points were calculated using the slope & intercept values calculated for the left / right separated points only. The new lines are starting from the bot of the image (image height) up to ~1/3. The results were OK-ish after some parameters tweaking again.

The lines returned from hough_lines were filtered initially using their slope (had to be between approx 0.5 ~ 0.8 and -0.5 ~ -0.8) but the results were not as expected. After that I tried to calculate the line's angle but still the filter doesn't work as expected.

## 2.Shortcomings
The shortcomings are better seen on the challenge video.

Main shortcomings are that this solution does not work very well when there are many "impurities" in the image, like tree shadows or various asphalt material/color changes. Also, steeper road curves will not be detected.

## 3. Improvements

A possible improvement would be to correctly detect the angle of the lines and consider to filter only those with angles very close to lane lines.

Other improvements could be in the tweaking of parameters. Currently, by manually selecting the parameters it takes a very long time to find the best ones (I probably spent ~50% or more project time only on tweaking the parameters). An interface where quickly changing the parameters using a slider for example and updating the results on an image close to realtime should prove very useful. Here the results could be tested on various images of "tricky" roads (with tree shadows or other kind of pavement lines for example) and quickly find better parameters for hugh lines, canny detector etc.
