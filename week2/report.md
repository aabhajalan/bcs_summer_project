# week2

1. The img.header lists all the information related to the nii image
2. some important info:  
pixdim: array that stores the physical size of each voxel, along each dimension (voxel spacing)  
dim: array that stores the dimensions of the image data   
srow_x, srow_y, srow_z: 3 affine transformation vectors, each with 4 elemtns, which together define the affine transformation matrix
3. the affine transformation matrix maps the voxel indices (i,j,k) to real world coordinates (x,y,z)
4. it includes rotation, scaling, shearing and translation, and the last row is added for homogeneous coordiantes
5. the aff2axcodes gives the orientation of the real-world axes, here it is R,A,S, ie, as the image axis 0, (rows) increases, the voxel moves to the right side of the patient
6. a better way to calcuate voxel spacing is by using the affine matrix (taking the euclidean norm of each column vector) as it takes into account rotaiton, shearing etc
7. to get the real world coordinates of any voxel:  
voxel=[i,j,k,1] --> 1 is added to homogenize the vector  
world= affine @ voxel
8. **Comparing NIfTI vs DICOM Orientation Handling**
---------------------------------------------------

| Feature                  | **NIfTI**                              | **DICOM**                                          |
|--------------------------|----------------------------------------|----------------------------------------------------|
| format                   | 3D/4D volume with a single header             |  set of 2D slices
| voxel positioning        | 4×4 affine matrix                  | Orientation vectors + position tags + spacing           |
| Rotation/Skew handling   | Fully encoded in affine             | Implicit via direction cosines                  |
| Slice spacing            | affine[:, 2] or get_zooms()       | distance between slice positions  |
| Coordinate system        | Typically RAS                      | Patient based (LPS)               |

9. **Comparing DICOM and NIFTI**
------------------------------------------------------
| **Aspect**                  | **DICOM**                                   | **NIfTI**                                 |
|--------------------------------|------------------------------------------------|-------------------------------------------|
| Intended Use**            | Clinical/medical imaging standard (used in hospitals, PACS systems)                        | Research and neuroimaging (e.g., fMRI, MRI, studies)                                       |
| Metadata Structure     | Complex, extensive — stores detailed patient, scanner, and acquisition info               | Minimal, focused — mainly includes spatial and orientation data in the affine + header    |
| File Format    | One file per slice (`.dcm`)              | Single file for full volume (`.nii` or `.nii.gz`)                                         |
| Ease of Use in Python   | More complex: requires DICOM parsing libraries (e.g., pydicom, dicom-numpy) | Much easier: nibabel directly loads volumes with affine and data in one step            |