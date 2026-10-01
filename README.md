# Drama Collective Stage Planner

An interactive 3D planner for theatre productions in the school hall, built with [three.js](https://threejs.org). Build the stage set, light it, seat the audience, then walk through the hall or save realistic pictures and 3D models.

It is a static web page with no build step. Every texture, backdrop and model is generated in code, so the folder is all you need to host it.

## What you can do

| Part | Details |
| --- | --- |
| **Hall** | About 18 m wide, 6.5 m high, 20 m from the stage front to the back wall. Black pleated curtains on every wall, blue tile floor, and a 6 × 8 grid of ceiling panel lights. |
| **Stage** | Fixed size: 7 m wide, 4 m deep, 1 m high. Blue carpet top and a black skirt. |
| **Stair blocks** | 0.8 × 0.8 × 0.8 m with four 0.2 m steps. Place them beside, in front of or on the stage, and stack them. |
| **Risers** | 1 × 1 × 0.5 m mobile decks that snap to the stage edges. Stack two to reach stage height (T-runways, thrusts, levels). |
| **LED wall** | Fixed 8 × 4.5 m screen behind the stage. Pick a built-in picture or upload your own. Its picture lights the stage in matching colours. |
| **Box sets** | Three-walled rooms (back, left and right walls) of printed flex sheets. Set the size, and choose or upload a different print for each wall. |
| **Tables and chairs** | Dining, round, desk and folding tables (with tablecloths). Wooden, plastic, armchair, stool and throne chairs. |
| **Lockers** | One steel unit, 0.9 × 1.8 × 0.5 m, sage powder-coat finish. Switch between 9 doors (3 × 3) and 15 doors (3 × 5). |
| **Floor spotlights** | RGB LED pars. Place them anywhere on the floor, the stage or a riser, and aim them anywhere. Set colour (picker, RGB values or gels), intensity, beam spread and diffusion. |
| **Lighting rig** | White box truss above the stage on chain hoists. Rig spotlights clamp anywhere on its bars, with the same controls as floor spotlights. |
| **General lights** | One switch for the ceiling panels, with a dimmer and colour temperature. |
| **Haze** | Makes light beams visible in the air. |
| **Speakers** | Tall column speakers, 2–2.5 m. Place them anywhere and set the direction and tilt. |
| **Audience** | Straight or curved rows in 1–4 blocks, with adjustable aisles, spacing and distance from the stage, plus optional red carpet aisles. Remove or restore single seats. |

Every part has presets, and there are whole-scene presets: Founder's Day, fashion show runway, talk or seminar, drawing-room drama, school play, concert night and an empty hall.

### Lighting

All lights are real light sources. Spotlights, rig lights, the LED wall and the ceiling panels all light and shade the scene. To keep it fast, only the brightest few spotlights cast shadows (0, 4 or 8 depending on the quality setting), and **Snapshot** always renders with 8 high-resolution shadows.

### Two modes

- **Design**: orbit the hall, click to select, drag to move. Items rest on whatever is below them: the floor, the stage, a riser or a stair block.
- **Walk through**: first-person view at eye height. Click to look around with the mouse, or drag to look. Use W A S D or the arrow keys to walk and Shift to hurry. Stairs and risers can be climbed; walls, sets and the stage front block you. Number keys 1–6 jump to viewpoints such as the front row, the back of the hall or on stage.

### Saving

- **Snapshot**: saves a PNG at HD, 1440p, 4K or portrait-post size.
- **File → Save design**: saves a `.json` file you can open again, including any uploaded pictures.
- **File → Export 3D model**: saves a `.glb` with lights, for Blender, SketchUp or any glTF viewer.
- The current design is also kept in the browser between visits.

## Keyboard shortcuts (design mode)

| Key | Action |
| --- | --- |
| R / Shift + R | Rotate the selection 90° (15° for lights and speakers) |
| Arrow keys | Nudge (hold Shift for bigger steps) |
| Page Up / Page Down | Raise or lower by 10 cm |
| Ctrl + D | Duplicate |
| Delete | Remove |
| Ctrl + Z / Ctrl + Shift + Z | Undo / redo |
| F | Frame the selection |
| Esc | Cancel placing, aiming or seat editing; clear the selection |

## Run it locally

Any static file server works. From this folder:

```bash
npx serve .
```

or

```bash
python -m http.server 8000
```

Then open the address it prints. Opening `index.html` straight from disk will not work, because browsers block ES modules on `file://`.

## Put it on a website

### GitHub Pages

In the repository on GitHub, open **Settings → Pages**. Under **Build and deployment**, choose **Deploy from a branch**, pick `main` and `/ (root)`, then save. After a minute the planner is live at `https://ar061290.github.io/dramacollective_stagerenders_v1/`.

### Embed it in another page

```html
<iframe
  src="https://ar061290.github.io/dramacollective_stagerenders_v1/"
  title="Stage planner"
  style="width: 100%; height: 80vh; border: 0;"
  allow="fullscreen; pointer-lock">
</iframe>
```

### Address options

| Option | Effect |
| --- | --- |
| `?scene=founders` | Start from a scene: `founders`, `runway`, `seminar`, `living`, `school`, `concert` or `empty` |
| `?design=URL` | Load a saved `.json` design from a URL (it must allow cross-origin requests) |
| `?mode=walk` or `#walk` | Start in walk-through mode |
| `?quality=low` | `low`, `medium` or `high` |
| `?embed` | Start with the parts panel closed |
| `?fresh` | Ignore the design saved in this browser |

For example, `index.html?scene=concert&mode=walk` opens the concert scene in walk-through mode.

## Code layout

```
index.html        page shell and three.js import map (three@0.170.0 from jsDelivr)
css/app.css       interface styles
js/main.js        renderer, post-processing, render loop, modes, camera views, saving
js/state.js       design state, undo/redo, save/load, browser storage
js/presets.js     presets for every part and the whole-scene presets
js/hall.js        hall, curtains, stage, ceiling lights
js/ledwall.js     LED wall and the light it throws
js/rig.js         lighting truss and clamping to its bars
js/seating.js     audience layout (instanced chairs) and carpet aisles
js/items.js       placeable item types and the scene sync
js/models.js      procedural models (stairs, risers, box sets, furniture, lockers, speakers, spotlights)
js/materials.js   materials and procedural textures
js/images.js      built-in LED and set pictures, uploads
js/interact.js    select, drag, place, aim, seat editing, shortcuts
js/walk.js        walk-through mode
js/ui.js          panels and controls
js/download.js    file saving (browser download, or the claude.ai downloads capability)
```

Units are metres. +x runs across the hall, +y is up, +z points from the stage toward the audience, and the stage front edge sits on z = 0.
