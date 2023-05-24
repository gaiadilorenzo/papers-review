# HybrIK: A Hybrid Analytical-Neural Inverse Kinematics Solution for 3D Human Pose and Shape Estimation

**Goal**: improve the step between 3D keypoint estimation and 3D mesh reconstruction.
**Problem**: learning the parameters directly leads to misalignment, learning pixel 3d keypoints position leads to unrealistic structure

## 3D Keypoint Estimation
### Related Work
Single-stage approaches drectly estimate the 3D joint locations from the input image. Various representations are developed, including 3D heatmapl, location-map, and 2D heatmap + z regression. Two-stage approaches first estimate 2D pose and then lift them to 3D joint locations by a learned dictionary of 3D skeleton or regression. Two-stage approaches highly rely on the accurate 2D pose estimators, which have achieved impressive performance by the combination of powerful backbone network and the 2D heatmap.

However, the human structural information is modelled implicitly by the neural network, which can not ensure the output 3D skeletons to be realistic
In the **hybrik approach** combine the approach of the 3D skeleton and parametric model.

## 3D Pose and Shape Estimation
### Related Work
Model-based 3D pose and shape estimation methods use parametric human body model as the output target because they capture the statistics prior of body shape.
 
## The model
The core of the approach is to calculate the relative rotation of the human bodyparts through a hybrid IK process. It combines the interpretable characteristics of the analytical solution and the flexibility of the twist and swing decomposition.

### Forward Kinematics 
**Forward kinematics** refer to the process of computing the reconstructed pose Q with the rest pose T and the relatove rotations.
```
    Q = FK(R, T)
```
### Inverse Kinematics

**Inverse Kinematics (IK)** is a technique used in robotics and computer graphics to determine the required joint angles of a robot or character in order to achieve a desired end-effector position and orientation. An Inverse Kinematic Model (IKM) is a mathematical representation of this process.

An IKM takes as input the desired position and orientation of the end-effector, as well as information about the robot's physical structure and joint limits. It then calculates the joint angles required to achieve the desired end-effector pose.
The FK problem is well posed as oposed to the IK problem which is ill posed.

Twist and swing decomposition. The twist is predicted by the neural network, the swing is the analytical solution. Each rotation is divdied in swinf and twist.

Why the twist?

The twist rotaion is rotating around t. This with t itself and phi the angle we can determine the twist rotation.
It needs to be learned through a neural network, however it has only 1DoF.

### Naive HybrIK
We retrieve the relative rotation R_{pa(k)}, k instead of the global rotations R_{k}. The problem with retrieving the global rotation is that the twist angle will depend
on all the ancestors rotation in the kinematic tree, which increases the variation of the distal limb joint and the capacity of the network to learn.

### Adaptive HybrIK
Assumption of naive hybrik is that the rest pose template and the predicted pose template have similar distances.
