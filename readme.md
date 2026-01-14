# Liveness Detection App

A single-page web application that verifies a real human is present using active challenges (blink, head movement, facial expressions) with MediaPipe Face Mesh for detection.

## Features

- **Blink Detection** - Eye Aspect Ratio (EAR) + blendshapes
- **Head Movement** - Turn left/right, look up/down (pose estimation)
- **Facial Expressions** - Smile, open mouth, raise eyebrows
- **Random Challenges** - 4 challenges selected randomly per session
- **Anti-Spoofing** - Multiple measures to prevent photo/video attacks

## Technology Stack

- Single HTML file with inline CSS and JavaScript
- **MediaPipe FaceLandmarker** - 478 facial landmarks + 52 blendshapes
- **WebRTC** for camera access
- No server required - runs entirely in the browser

## Quick Start

1. Open `index.html` in a modern browser (Chrome, Firefox, Safari)
2. Grant camera permission when prompted
3. Position your face in the oval guide
4. Complete 4 random challenges
5. Get verification result

## How It Works

### Challenge Types

| Challenge | Detection Method | Threshold |
|-----------|-----------------|-----------|
| Blink | EAR < 0.21 or blendshape > 0.5 | 2+ blinks |
| Head turn left/right | 10° yaw from baseline | Hold 500ms |
| Head tilt up/down | 8° pitch from baseline | Hold 500ms |
| Smile | blendshape > 0.4 | Hold 500ms |
| Open mouth | blendshape > 0.3 | Hold 500ms |
| Raise eyebrows | blendshape > 0.3 | Hold 500ms |

### Anti-Spoofing Measures

- Random challenge order each session
- Unpredictable timing between challenges
- Multiple detection types combined
- Hold duration requirements
- Continuous face presence validation
- 3D depth variance analysis
- Movement pattern analysis

## Browser Support

- Chrome (recommended)
- Firefox
- Safari
- Edge

Requires WebGL support for GPU-accelerated face detection.

## Development

The application is contained in a single `index.html` file with:

- **BlinkDetector** - EAR calculation + blendshape detection
- **HeadMovementDetector** - Yaw/pitch estimation with auto-calibration
- **ExpressionDetector** - Smile, mouth open, eyebrow detection
- **ChallengeManager** - State machine for verification flow
- **AntiSpoofingModule** - Multiple spoofing detection methods

## License

MIT License

Copyright (c) 2025

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
