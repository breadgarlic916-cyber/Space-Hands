# Space-Hands
This is a project heavily inspired off space station 13, but is meant to have a different aesthetic, perspectives, progression, and is also meant to be single player with NPC's playing the other players. It's more of a solo challenge and the mechanics won't all be the same, as well as my own added mechanics, features, and possibly lore.

-Engine & Rendering
    -GameWindow - Wraps the native OS window (OpenTK/SDL), handles context creation and buffer swapping.
    -RenderLoop - The high-precision loop that decouples update logic (Simulation) from draw logic (Frame rate).
    -ShaderManager - Compiles and links GLSL/HLSL shaders, manages uniform locations.
    -TextureManager - Loads images from disk, handles texture atlases for isometric sprites.
    -MeshBuilder - Dynamically generates vertices for isometric cubes and walls.
    -Camera3D - Manages the view matrix, projection matrix (Orthographic for Iso), and zoom levels.
    -BatchRenderer - Optimizes draw calls by grouping sprites/meshes sharing the same texture.
    -InputPoller - Polls raw keyboard/mouse state from the OS window.

-Root & Loop
    -Bootstrap - The entry point (Main) that initializes the Engine and then the Simulation.
    -GameManager - The singleton state machine (Intro -> MainMenu -> Loading -> RoundActive).
    -RoundManager - Manages the round timer, win conditions, and mode switching.
    -SettingsManager - Loads/Saves resolution, volume, and keybinds config files.

-Core Simulation
    -Atom - The god class. Base for all physical entities. Handles UID and Coordinates.
    -Turf - Represents a static grid tile. Handles friction and gas permeability.
    -Area - A defined zone of Turfs (e.g., Medbay). Handles local gravity and lighting state.
    -Structure - Base for walls/windows. Handles density and structural integrity.
    -IsoGrid - The map data structure. Converts World Coordinates to Grid Indices.

-Entities & AI
    -Entity - Base class for any object that can move or be moved.
    -Mob - Base class for living creatures. Handles movement intentions and status effects.
    -Humanoid - Complex mob with body parts, species definitions, and equipment slots.
    -Ghost - Spectator controller for dead players with "Respawn Queue" logic.
    -UserProfile - Persistent data: Rank, XP, Unlocked Roles (Saved to Disk).
    -RarityGenerator - The logic engine that promotes Commons to Rares to Legendaries.
    -AIController - The brain class. Holds the State Machine for NPC behavior.
    -AIBehaviorTree - The decision logic nodes (Sequence, Selector, Task).
    -NameDatabase - Parses JSON to assign names based on Rarity/Dept logic.
    -AntagonistController - Assigns traitor objectives and special gear access.

-SS13 Systems
    -HealthManager - Manages BodyParts (Limbs/Organs) and DamageTypes (Brute/Burn/Tox/Oxy).
    -InventoryManager - Manages UI slots (Back/Belt/ID/Hands) and item swapping.
    -InteractionController - Raycasts mouse clicks to World. Determines "Use" vs "Attack".
    -AtmosManager - Global controller. Iterates active Turfs to equalize gas pressure/temp.
    -PowerNet - Global controller. Graph search for Cables connecting Sources to Loads.
    -SpaceVoidSystem - Checks for Mobs on space tiles without jetpacks -> Triggers instant death.

-Engineering & Power
    -Cable - Physical wire entity. Can be pulsed, cut, or shocked.
    -AreaPowerController - Room battery/switch. Charges from Net, powers Room.
    -PowerSmes - High-capacity station battery for grid smoothing.
    -PowerGenerator - Base class for energy sources (Solar/Combustion/Singularity).
    -TelecommsManager - Routes chat messages. Checks for active Server/Relays.
    -TelecommsServer - Machine logic. Stores encryption keys and logs.

-Medical & Chemistry
    -ReagentContainer - Attached to containers/organs. Holds list of Reagents + Volume.
    -Reagent - Data class defining chemical properties and metabolic effects.
    -MedicalScanner - UI readout showing target's BodyPart damage and overdose status.
    -CloningPod - Machine. Scans records and prints new bodies (minus defects).
    -ChemDispenser - Machine. Spawns raw elements (Carbon, Sugar, Radium).
    -GeneticsConsole - UI for manipulating the Genome string.
    -Genome - Data class defining mutations (Powers/Defects) and appearance.

-Science & Research
    -ResearchConsole - UI Tree for unlocking TechNodes with points.
    -TechNode - Data class for a specific technology unlock.
    -Protolathe - Machine. Prints items based on Unlocked TechNodes.
    -DestructiveAnalyzer - Machine. Destroys items to generate Research Points.
    -XenobioSlime - Mob. Handles splitting, color evolution, and extract generation.

-Supply, Service & Cargo
    -CargoConsole - UI for spending Credits on crates.
    -SupplyShuttle - Logic moving the shuttle Area between Z-levels.
    -HydroponicsTray - Machine. Manages water/nutrient values for PlantSeeds.
    -KitchenMicrowave - Crafting station. Converts raw ingredients to Food items.
    -MiningDock - (Placeholder) Logic for the lavaland shuttle transport.

-Security & Access
    -AccessReader - Component. Checks IDCard tags against Door requirements.
    -IDCardConsole - Machine. Writes new access tags to IDCards.
    -SecurityRecord - Data entry linking character Name to Arrest Status.
    -BrigTimer - Logic for auto-locking/unlocking doors after a duration.

-Input & UI
    -InputController - Maps Hardware Input to Game Actions (WASD -> Iso Vector).
    -IsoCamera - Logic for camera position, zoom, and wall occlusion transparency.
    -DialogueManager - Filters dialogue options based on Target Context/Subcontext.
    -HotkeyDialogueManager - Stores and executes player-defined chat macros.
    -RandomEventDirector - Scheduler. Rolls for disasters (Meteor/Blob) based on chaos.

-Items
    -Item - Base class for pickup-able objects. Handles Weight/Size.
    -Container - Item holding other items. Handles nesting logic.
    -Tool - Item with specific "Use" functionality (Wrench/Welder).
    -Weapon - Item with combat stats (Force/AttackSpeed).
    -IDCard - Item storing Name, Job, and Access Tags.
