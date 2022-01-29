# optical-flow-attack

The idea of this research was to try to break the optical flow architectures using noise patch

Architecture | Dataset the model was trained         | Dataset the attack was evaluated | MSE with squared box as noise with shape 50*50 | MSE with squared box as noise with shape 100*100 | MSE with squared box as noise with shape 150*150 | MSE with squared box as noise with shape 200*200 |
--- |---------------------------------------|------------------------------|-----------------------------------------------------|-------------------------------------------------------|-------------------------------------------------------|-------------------------------------------------------|
PWC-Net | FlyingChairs + FlyingThing3d subset + Sintel + KITTI2015 + HD1K                                | Kittie                   | 0.336                                                 | 1.186                                                   | 8.767                                                   | 19.185                                                   
FlowNet2 | FlyingThing3d subset                  | Kittie                         | 0.174                                                 | 0.606                                                   | 2.006                                                   | 6.653                                                   
RAFT | Mixed of FlyingChairs, FlyingThing3d, Sintel, KITTI2015, and HD1K                             | Kittie                             | 5.141                                                 | 2.456                                                   | 9.001                                                   | 15.201                                                   
LightFlowNet2 | Flying Chairs + Flying Thing3d subset |       Kittie                       | 0.275                                                 | 1.063                                                   | 5.330                                                   | 13.639                                                   

### The evaluation idea:
* Calculate flow between two raw images.  
* Calculate flow between two images with noise.
* Set noise pixels to zero.
* Calculate differences between backgrounds (MSE)

### MSE function I used to compare images

```python
def MSE(imageA, imageB):
    err = np.sum((imageA.astype("float") - imageB.astype("float")) ** 2)
    err /= float(imageA.shape[0] * imageA.shape[1])
    return err
```
### Example of attacked PWC-Net with box size 200*200
https://drive.google.com/file/d/1-o498xf7k6IG-sUR5TGoYc83STzTiXpt/view?usp=sharing

### Example of attacked FlowNet2 with box size 200*200
https://drive.google.com/file/d/100xqDbEy8tz3ucRYmKlMS__hcujH3byP/view?usp=sharing

### Example of attacked RAFT with box size 200*200
https://drive.google.com/file/d/10Twuh4wPMHYjt-iDEcSibWvgh5GAM69V/view?usp=sharing

### Example of attacked LightFlowNet2 with box size 200*200
https://drive.google.com/file/d/10akdwT6Y_p6a45BxuL4T34FKk7QylkEg/view?usp=sharing

### Conclusions
Based on my investigation FlowNet2 was the most stable to the attack. PWC-net had the worst performance. It looks like the network tried
to interpolate noise part. 
