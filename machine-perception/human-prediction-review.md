### Convolutional Autoencoders for Human Motion Infilling
## Task
Motion infilling for 3D human motion data.

## Proposed Solution 
Treat motion infilling as an inpainting problem and to train a convolutional de-noising autoencoder on image-like representations of motion sequences. It proposes variations of the idea to combine l1 or l2 penalized auto-encoders with an adversarial loss in order to synthesize realistic looking nat- ural images

## Challenges
Motion infilling is challenging as it requires learning smooth transitions between possibly different types of motion. RNN have been used for this task, although they are notoriously difficult to condition for future sequences. Regarding this bi-directional structure have been used to support this, although the supported gap size is fixed and short.
Using CNN avoids the issue caused by recurrent models. Although the problem arising when using such representations is that neighboring “pixels” are not necessarily neighboring joints in the skeletal hierarchy (some solutions are, dense layersspanning the entire height, large kernel sizes, 1d convolutional layer over the temporal domain only).

## Related Works
- `RNN` .
    - Issues: 
        1. It stucks to a mean pose (sol: perturb the input at test time with Gaussian noise, removing joints entirely, training the model on its own outputs, or employing a specialized output layer that follows the kine- matic chain of the skeleton)
        2. Discontinuity between the last known and first predicted pose (sol: a residual connection in a sequence-to-sequence architecture,  employing adversarial and geodesic-inspired losses.)
        3. In addition to temporal dependencies, also highly complex spatial dependencies related to the dynamics of human motion must be captured.
- `Non Reccurent models`:
    - Graph convolutional network (GCN). The GCN operates on discrete cosine transforms of the input data, which makes it more difficult to support variable length inputs
    - Convolutional models deal with the fact that neighboring values in the data matrix are not necessarily neighboring joints in the human skeleton.

## Training and Evaluation
This dataset represents poses as 3D positions of joints in space. 
In the proposed method, a curriculum learning scheme is used to enable prediction over variable gap length and to achieve robustness against various forms of noise in the inputs. During training, we mask increasingly large blocks of the input data so that the model is forced to learn how to generate plausible pose data to fill in the gaps. The method has been evaluated in a number of experiments to illustrate its capabilities but also to identify its limits.
The loss is the `1 loss between each pair of original and reconstructed samples (Xi, Xi-hat). We found that the `1 loss results in smoother reconstructions when compared to the `2 loss, which reflects previous work on image inpainting. Please note that the loss is computed over the entire motion sequence, not just the masked portion. In this way, the model is forced to learn the global structure of the motion. 

In summary, the paper proposes a method that uses convolutional autoencoders for human motion infilling. This approach shows great potential in generating plausible and coherent motion data for a wide range of activities. The proposed method can handle variable gap lengths and is resistant to different forms of noise in the inputs. Several experiments were conducted to evaluate the effectiveness of the proposed method, and it was found to outperform previous state-of-the-art methods in terms of both quantitative metrics and visual quality. However, the study also identified some limitations of the proposed method, such as difficulty in generating fine-grained details and occasional instability during training. Overall, this work is considered to be an important step towards more realistic and effective generation of human motion data using deep learning techniques.