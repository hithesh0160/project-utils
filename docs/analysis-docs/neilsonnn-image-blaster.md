# Image Blaster (`neilsonnn/image-blaster`)

[Image Blaster](https://github.com/neilsonnn/image-blaster) (by [neilsonnn](https://github.com/neilsonnn)) is an open-source, image-to-3D-world generative pipeline and agent skill designed for Claude Code.

It converts a single 2D image or photo into a fully interactive, walkable 3D environment complete with 3D mesh objects and spatial audio in under 5 minutes.

---

## Generative Pipeline Architecture

Image Blaster orchestrates multiple specialized AI models into a unified agent workflow:

- **World Labs (Marble 1.1):** Generates a Gaussian splat (`.spz`) representation of the static environment background for a walkable space.
- **Hunyuan 3D (via FAL API):** Extracts key foreground objects from the 2D photo and generates dynamic 3D asset meshes (`.glb`, `.obj`).
- **Nano-banana:** Performs image segmentation and source plate cleanup.
- **ElevenLabs API:** Generates ambient spatial background audio loops and physics sound effects (`.mp3`).

```text
       Input Photo (input/image.jpg)
                     │
                     ▼
          Claude Code Agent Workflow
       ┌─────────────┼─────────────┐
       │             │             │
       ▼             ▼             ▼
  World Labs      Hunyuan 3D    ElevenLabs
 (Gaussian Splat) (3D Meshes)  (Spatial Audio)
       └─────────────┬─────────────┘
                     │
                     ▼
   Exported 3D Environment (.spz / .glb / .mp3)
  (Compatible with Unity, Unreal, Godot, Blender, Three.js)
```

---

## Export Compatibility

Output assets can be imported into major 3D engines and modeling tools:
- **Game Engines:** Unity, Unreal Engine 5, Godot.
- **3D Software:** Blender, Maya, 3ds Max.
- **Web 3D:** Three.js / WebXR.

---

## Agent Usage

1. Place target image in `input/`.
2. Prompt Claude Code: `"blast it and confirm each step with me"`.
3. Agent coordinates model calls, verifies step outputs, and outputs the packaged 3D scene.

---

## Resources

- **GitHub Repository:** [neilsonnn/image-blaster](https://github.com/neilsonnn/image-blaster)
- **License:** Open Source
