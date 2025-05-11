# A-LOAM ROS Node Wrapper

This repository provides a ROS node wrapper for A-LOAM (Advanced Lightweight Odometry and Mapping), integrating three core modules in one node: Scan Registration, Laser Odometry, and Laser Mapping. The implementation is based on [A-LOAM](https://github.com/HKUST-Aerial-Robotics/A-LOAM) by HKUST Aerial Robotics Group.

## Configuration

All parameters are configured in `config.txt`:

### Scan Registration Configuration
```plaintext
scan_line=32                    # Number of scan lines (e.g., 32 for Velodyne-32)
minimum_range=0.1               # Minimum valid range for point filtering
pub_each_line=false            # Whether to publish each scan line
input_pointcloud_topic=/kitti/velo/pointcloud  # Input point cloud topic
```

Optional: Custom angle configuration is available by uncommenting and modifying the `angles` parameter:
```plaintext
# angles=10.67,9.33,8.0,6.67,5.33,4.0,2.67,1.33,0.0,-1.33,-2.67,-4.0,-5.33,-6.67,-8.0,-9.33,-10.67,-12.0,-13.33,-14.67,-16.0,-17.33,-18.67,-20.0,-21.33,-22.67,-24.0,-25.33,-26.67,-28.0,-29.33,-30.67
```

### Laser Odometry Configuration
```plaintext
skip_frame=2                    # Number of frames to skip between processing
distortion=false               # Whether to compensate for motion distortion
```

### Laser Mapping Configuration
```plaintext
line_resolution=0.4            # Resolution for line feature matching
plane_resolution=0.8          # Resolution for plane feature matching
```

## Usage

### Example Code
```cpp
#include <iostream>
#include "aloam_wrapper.h"

int main(int argc, char** argv) {
    if (argc != 4) {
        std::cout << "Usage: " << argv[0] << " <bag_path> <config_file_path> <output_dir>" << std::endl;
        return 1;
    }

    try {
        std::string bag_path = argv[1];
        std::string config_path = argv[2];
        std::string output_dir = argv[3];

        runALOAM(bag_path, config_path, output_dir);
    } catch (const std::exception& e) {
        std::cerr << "Error: " << e.what() << std::endl;
        return 1;
    }

    return 0;
}
```

### Running the Node
```bash
./your_executable path_to_bag config.txt output_directory
```

The program requires three command-line arguments:
1. `bag_path`: Path to the ROS bag file containing LiDAR data
2. `config_file_path`: Path to the configuration file
3. `output_dir`: Directory for storing output files

## Acknowledgments

This work is based on [A-LOAM](https://github.com/HKUST-Aerial-Robotics/A-LOAM) by HKUST Aerial Robotics Group. We thank the original authors for their outstanding work on lightweight LiDAR odometry and mapping.

## License
This project is under the same license as A-LOAM. Please refer to the original repository for license details.

