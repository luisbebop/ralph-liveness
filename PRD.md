# Liveness Detection App - Implementation Plan

## Overview
Single-page HTML application that verifies a real human is present using active challenges (blink, head movement, facial expressions) with MediaPipe Face Mesh for detection.

## Technology Stack
- **Single HTML file** with inline CSS and JavaScript
- **MediaPipe FaceLandmarker** (CDN) - 478 facial landmarks + 52 blendshapes
- **WebRTC** for camera access

## Architecture

```
+------------------------------------------+
|            Single HTML File               |
|  +------------------------------------+  |
|  |  HTML: Video, Canvas, UI elements  |  |
|  +------------------------------------+  |
|  |  CSS: Modern styling, animations   |  |
|  +------------------------------------+  |
|  |  JavaScript Modules:               |  |
|  |  - BlinkDetector                   |  |
|  |  - HeadMovementDetector            |  |
|  |  - ExpressionDetector              |  |
|  |  - ChallengeManager (state machine)|  |
|  |  - UI Controller                   |  |
|  +------------------------------------+  |
+------------------------------------------+
```

## Challenge Types
1. **Blink detection** - Eye Aspect Ratio (EAR) + blendshapes
2. **Head movement** - Turn left/right, look up/down (pose estimation)
3. **Facial expressions** - Smile, open mouth, raise eyebrows
4. **Random sequence** - 4 challenges selected randomly per session

## Key MediaPipe Resources
- CDN: `https://cdn.jsdelivr.net/npm/@mediapipe/tasks-vision/vision_bundle.js`
- Model: `https://storage.googleapis.com/mediapipe-models/face_landmarker/face_landmarker/float16/1/face_landmarker.task`

## Implementation Steps

### Phase 1: HTML Structure & CSS
1. Create responsive HTML layout (video, canvas overlay, UI panels)
2. Style with CSS variables, flexbox, animations
3. Add face position guide (oval overlay)

### Phase 2: MediaPipe Integration
4. Load FaceLandmarker from CDN with GPU delegate
5. Set up WebRTC camera (getUserMedia, mirrored view)
6. Create detection loop with requestAnimationFrame

### Phase 3: Detection Modules
7. **BlinkDetector** - EAR calculation + blendshape eyeBlinkLeft/Right
8. **HeadMovementDetector** - Yaw/pitch from nose tip vs eye positions
9. **ExpressionDetector** - Smile, mouth open, eyebrow raise via blendshapes

### Phase 4: Challenge State Machine
10. States: INITIALIZING → CALIBRATING → PRESENTING → DETECTING → SUCCESS/FAILURE → COMPLETED
11. Random challenge generation with variety enforcement
12. Timeout handling (5s per challenge) + hold duration (500ms)

### Phase 5: UI & Feedback
13. Challenge instructions with icons and hints
14. Progress indicators (completed challenges, hold progress bar)
15. Success/failure animations and final result screen

### Phase 6: Polish
16. Face mesh visualization (contours, feature highlights)
17. Error handling (camera denied, face not detected)
18. Anti-spoofing measures (randomization, continuous tracking)

## Anti-Spoofing Features
- Random challenge order each session
- Unpredictable timing between challenges
- Multiple detection types (blink + movement + expressions)
- Hold duration requirements
- Continuous face presence validation

## Key Detection Thresholds
| Detection | Threshold | Method |
|-----------|-----------|--------|
| Blink | EAR < 0.21 or blendshape > 0.5 | 2+ consecutive frames |
| Head turn | 15° yaw from baseline | Hold 500ms |
| Head tilt | 12° pitch from baseline | Hold 500ms |
| Smile | blendshape > 0.4 | Hold 500ms |
| Mouth open | blendshape > 0.3 | Hold 500ms |
| Eyebrow raise | blendshape > 0.3 | Hold 500ms |

## Verification
1. Open `index.html` in Chrome/Firefox/Safari
2. Grant camera permission
3. Position face in oval guide
4. Complete 4 random challenges
5. Verify success screen shows for real faces
6. Test with photo/video to confirm rejection

## File to Create
- `/Users/luisbebop/Documents/ai/ralph-liveness/index.html` - Complete single-page app
