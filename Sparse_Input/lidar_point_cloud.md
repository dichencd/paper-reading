# sparse input
- one key difference: detection (fact) VS generation (non-fact)
  - maybe this explains why interpolation in detection is okay -- we are interpolating the target object itself.
- do detection and generation have the same required resolution? --our requirement is higher
- detection result pedestrain/cyclist is the worst
- non uniform sampling density (similar phenomenon, but do they have the same essence?)
- pseudo image establish spatial relationship
- use points + max pool improve robustness
- important: points do not contain polyline-level info, they don't need it
## xy
### pointnet & pointnet++
- x,y position as input
- for classification & segmentation
- cannot do object detection

## pseudo image + xy(z)
absolution position + relative position
### voxelnet (3d)

### pointpillar (2d)
- for object detection

