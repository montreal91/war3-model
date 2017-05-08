# war3-model
TypeScript-based mdl/mdx (Warcraft 3 model formats) converter/renderer

## Demo
* [MDL/MDX converter (also json-like structure-previewer)](https://4eb0da.github.io/war3-model/convert.html)
* [WebGL model previewer (png/jpg textures requires second drag&drop; blp not supported)](https://4eb0da.github.io/war3-model/preview.html)
* [BLP previewer (BLP1 decoder only)](https://4eb0da.github.io/war3-model/decodeblp.html)

## Usage
```bash
npm i war3-model --save
```

MDL parsing/generation
```typescript
import {parse as parseMDL} from 'war3-model/mdl/parse';
import {generate as generateMDL} from 'war3-model/mdl/generate';

let model = parseMDL('...');
let mdl = generateMDL(model);
console.log(mdl);
```

BLP => PNG node.js converter
```typescript
import * as fs from 'fs';
import {PNG} from 'pngjs';
import {decode, getImageData} from 'war3-model/blp/decode';

let blp = decode(new Uint8Array(fs.readFileSync(process.argv[2])).buffer);
let imageData = getImageData(blp, 0);
let png = new PNG({width: blp.width, height: blp.height, inputHasAlpha: true});

// I don't know how to create PNG with existing data, so...
for (let i = 0; i < imageData.data.length; ++i) {
    png.data[i] = imageData.data[i];
}

fs.writeFileSync('out.png', PNG.sync.write(png));
```

## MDL/MDX support
* All standart features like Sequences, Bones, Cameras, etc
* Multiple texture chunks (mdx only)
* Multiple sequences/nodes with the same name (not quite sure is it feature or not, but War3 actually contains such models)
* SoundTrack not supported

## Renderer support
* Standart geometry/animation
* Custom team color setting
* ReplaceableId 1/2
* Global sequences
* Alpha blending and multiple layers
* TextureAnimation
* Billboarded/BillboardedLockXYZ, w/o DontInherit/CameraAnchored
* RibbonEmitter (w/o Gravity and TextureSlot/Color animation)
* ParticleEmitter2 (with Tail/Head/Both/Squirt(?))
* No light support (normals, light, Unshaded, etc)
* No render priority support (PriorityPlane and others)

## BLP support
* BLP1 only (not BLP0 and BLP2 support)
* Decoder only, no encoder
* Direct & jpeg data
* Variable alpha (8/4/1/0 bit, but tested only 8/0)
* API for getting all mipmap level's data

## Thanks
* Magos (MDX specification, War3 Model Editor app/source)
* GhostWolf (aka flowtsohg) (MDX specification, BLP decoder code)
* Алексей (MdlVis app/source)
* Dr Super Good (BLP specification)

## Licence

This library uses modified version of modified version of jpgjs, JavaScript jpeg decoder.
Original modifications was made by @flowtsohg.

/*
 Copyright 2011 notmasteryet

 Licensed under the Apache License, Version 2.0 (the "License");
 you may not use this file except in compliance with the License.
 You may obtain a copy of the License at

 http://www.apache.org/licenses/LICENSE-2.0

 Unless required by applicable law or agreed to in writing, software
 distributed under the License is distributed on an "AS IS" BASIS,
 WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 See the License for the specific language governing permissions and
 limitations under the License.
 */
