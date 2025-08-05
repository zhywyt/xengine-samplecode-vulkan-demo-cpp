# Shading Rate Visualization Feature

This document describes the new shading rate visualization feature added to the VulkanExample.

## Overview

The shading rate visualization feature allows you to dynamically toggle between normal rendering and a visualization mode that displays the Variable Rate Shading (VRS) shading rate image directly on the screen.

## How it works

### Normal Rendering Mode
- VRS computation outputs to the shading rate image buffer
- The shading rate image is used by the light pass for variable rate shading
- Upscaling processes the light buffer to create the final high-resolution image
- The final image shows the normal rendered scene

### Visualization Mode  
- VRS computation outputs directly to the final screen image buffer
- Upscaling is skipped to preserve the shading rate visualization
- The final image shows the shading rate values as a visual overlay

## API Usage

### C++ API
```cpp
// Enable shading rate visualization
vulkanExample->SetVisualizeShadingRate(true);

// Disable shading rate visualization (normal rendering)
vulkanExample->SetVisualizeShadingRate(false);
```

### JavaScript/TypeScript API (via NAPI)
```typescript
// Enable shading rate visualization
this.xComponentContext.setShadingRateVisualization(true);

// Disable shading rate visualization
this.xComponentContext.setShadingRateVisualization(false);
```

### UI Controls
The sample app includes a checkbox in the UI labeled "Visualize Shading Rate" that allows real-time toggling of this feature.

## Technical Implementation

### Key Changes Made:
1. **Added toggle variable**: `visualize_shading_rate` controls the visualization mode
2. **Modified DispatchVRS function**: When visualization is enabled, `outputShadingRateImage` is set to `upscaleFrameBuffers.upscale.color.view` (the final screen image)
3. **Skip upscaling**: When visualization is active, upscaling is skipped to preserve the shading rate data
4. **Command buffer rebuilding**: The render loop automatically rebuilds command buffers when the visualization state changes

### Files Modified:
- `entry/src/main/cpp/render/model_3d_sponza.h` - Added visualization toggle variables and public API
- `entry/src/main/cpp/render/model_3d_sponza.cpp` - Implemented visualization logic in DispatchVRS and BuildUpscaleCommandBuffers
- `entry/src/main/cpp/render/plugin_render.h` - Added NAPI function declaration
- `entry/src/main/cpp/render/plugin_render.cpp` - Implemented NAPI function and added to exports
- `entry/src/main/ets/pages/Index.ets` - Added UI checkbox for toggling the feature

## Requirements

- VRS must be enabled (`use_vrs = true`) for the visualization to take effect
- The upscale rendering path must be active (`use_method != 0`) as visualization is implemented in `BuildUpscaleCommandBuffers`

## Usage Example

1. Enable VRS using the existing "useVRS" checkbox
2. Select an upscale method (spatial upscale or FSR upscale)
3. Check the "Visualize Shading Rate" checkbox to see the shading rate visualization
4. Uncheck it to return to normal rendering

The feature provides a powerful debugging and analysis tool for understanding how Variable Rate Shading is being applied to different regions of the rendered scene.