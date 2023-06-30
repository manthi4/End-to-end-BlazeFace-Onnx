# End-to-end-BlazeFace-Onnx
## Acknowledgement
I took some code for displaying images and the anchors.npy file from https://github.com/hollance/BlazeFace-PyTorch/tree/master <br>
In that repo hollance did good work in converting BlazeFace into a pytorch model.

## Descruption
A big problem with using MediaPipe's BlazeFace model and many other detection models is that they don't actually output the bounding box coordinates. Instead they output some annoying combination of anchor transformations and confidence scores. 
So when using these models we need to build post-processing such as scaling and non-max suppression in order to get useful output. An example of this being done in python can be found at https://github.com/hollance/BlazeFace-PyTorch/tree/master.
But for ONNX models that are meant to be used on edge devices or be exported to other languages this post-processing can be complicated to implement. So I converted the post processing into a series of matrix operations and appended them to the original BlazeFace Onnx model to mak a truly end-to-end version.

## Use
Look in the notebook for example uses.
* Inputs
> ``mpipe_bface_boxes_ops16.onnx' ``Takes input of (batch size, 128, 128, 3) as well as detection confidence threshold, IOU threshold, and maximum detections <br>
> ``T_mpipe_bface_boxes_ops16.onnx`` Takes input of (batch size, 3, 128, 128) as well as detection confidence threshold, IOU threshold, and maximum detections <br>
* Outputs
> (batch size, number of detection, 16)
> (box_top_L_Y, box_top_L_X, box_bottom_R_Y, box_bottom_R_X, L_eye_Y, L_eye_X, R_eye_Y, R_eye_X, Nose_Y, Nose_X, Mouth_Y, Mouth_X, L_ear_Y, L_ear_X, R_ear_Y, R_ear_X)<br>
> The XY coords are outputs in a range between [0, 1]. Make sure to scale them by the original image width and height respectively<br>
