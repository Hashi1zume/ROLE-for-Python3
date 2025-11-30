# ROLE for Python3
Origina ROLE : https://github.com/ricky40403/ROLE


Raindrop on lens effect

## 📸 Before and After

| Original Image | With Raindrop Effect |
|----------------|---------------------|
| ![Original](datasets/000030973.jpg) | ![With Raindrops](Output_image/000030973.jpg) |

## 🚀 Quick Start

### Prerequisites

- Python 3.11+
- Anaconda (recommended for dependency management)

### Installation

*  **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

### Basic Usage

```python
from raindrop.dropgenerator import generateDrops
from raindrop.config import cfg

# Generate raindrop effect on image
output_image = generateDrops('path/to/your/image.jpg', cfg)
output_image.save('output_with_raindrops.jpg')

# Generate with label map
cfg["return_label"] = True
output_image, label_map = generateDrops('path/to/your/image.jpg', cfg)
```

### Run Example

```bash
# Place images in datasets/ directory
python example.py
# This will generate both processed images and label maps
# Images saved to: Output_image/
# Labels saved to: Output_label/
```


## ⚙️ Configuration

Customize droplet generation in `raindrop/config.py`:

```python
cfg = {
    'maxR': 50,              # Maximum droplet radius (pixels)
    'minR': 30,              # Minimum droplet radius (pixels)
    'maxDrops': 30,          # Maximum number of droplets
    'minDrops': 30,          # Minimum number of droplets
    'edge_darkratio': 0.3,   # Edge darkening intensity (0.0-1.0)
    'return_label': False,   # Return segmentation labels
    'label_thres': 128,      # Label threshold for custom inputs
    'shape_variety': True,   # Enable random droplet shapes
    'allowed_shapes': [      # Available droplet shapes
        'default',           # Original teardrop (circle + ellipse)
        'round',             # Perfect circle
        'oval',              # Oval with random orientation
        'teardrop',          # Enhanced teardrop with variation
        'irregular',         # Irregular shape with distortions
        'splash'             # Splash-like with satellite droplets
    ]
}
```

## 🔧 Advanced Usage

### Custom Droplet Positions

```python
from PIL import Image
import numpy as np

# Create custom label map
custom_label = Image.open('custom_droplet_mask.png')
output_image, label = generateDrops('image.jpg', cfg, inputLabel=custom_label)
```

### Batch Processing

```python
import os
from raindrop.dropgenerator import generateDrops
from raindrop.config import cfg

input_dir = 'input_images/'
output_dir = 'output_images/'

for filename in os.listdir(input_dir):
    if filename.lower().endswith(('.png', '.jpg', '.jpeg')):
        input_path = os.path.join(input_dir, filename)
        output_image = generateDrops(input_path, cfg)
        output_path = os.path.join(output_dir, filename)
        output_image.save(output_path)
```

### Parameter Optimization

```python
# Light rain effect
cfg_light = cfg.copy()
cfg_light.update({'maxDrops': 15, 'minDrops': 10, 'maxR': 30})

# Heavy rain effect  
cfg_heavy = cfg.copy()
cfg_heavy.update({'maxDrops': 50, 'minDrops': 40, 'maxR': 70})

# Subtle effect
cfg_subtle = cfg.copy()
cfg_subtle.update({'edge_darkratio': 0.1, 'maxR': 25})
```

### Shape Variety (New Feature!)

```python
# Enable random droplet shapes
cfg_variety = cfg.copy()
cfg_variety.update({
    'shape_variety': True,  # Enable shape randomization
    'allowed_shapes': ['default', 'round', 'oval', 'teardrop', 'irregular', 'splash']
})

# Only circular droplets
cfg_round = cfg.copy()
cfg_round.update({
    'shape_variety': True,
    'allowed_shapes': ['round']
})

# Splash and irregular only
cfg_dynamic = cfg.copy()
cfg_dynamic.update({
    'shape_variety': True,
    'allowed_shapes': ['splash', 'irregular']
})

result = generateDrops('image.jpg', cfg_variety)
```
