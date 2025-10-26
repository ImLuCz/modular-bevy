# The Modular Bevy Project Architecture
This project structure is designed to make Bevy-based games modular, scalable, and easy to maintain.

## Design Goals

- **Modularity** – Each gameplay feature is isolated in its own folder with local components, systems, and logic files.

- **Scalability** – Adding or removing modules doesn’t affect others; you can extend the project easily.

- **Maintainability** – Clear file boundaries reduce coupling and make debugging simpler.

- **Reusability** – Modules can be reused in other projects or refactored independently.

While there very well may be cases where straying from the structure of this architecture can make sense,
it can also be a good indicator of whether your code adheres to these principles and, in turn, can be *easily understood, fixed and expanded upon*.

## Folder Responsibilities

### `core/`

Contains engine-level setup and shared game state.

`resources.rs` – Global resources (config, constants, assets).

`setup.rs` – Startup logic like window setup, camera spawning, player spawning...

Ideally, when starting a new project, this folder could be reused with minimal changes.

### `game/`

Contains all gameplay-specific modules.
Each submodule handles a distinct feature:

`components.rs` – Entity components.

`systems.rs` – Core systems tied to the module.

Optional specialized files (like `physics.rs`, `controller.rs`) to isolate complex logic.

Rule of thumb: shared →  `core/`, module-specific →  `game/module`.

## Why This Architecture

This architecture is designed to integrate and make sense with Bevy Engine's **ECS** (Entity Component System) approach to game development. 

- Keeps ECS data and logic localized.

- Encourages plugin-based composition.

- Scales well as projects evolve from simple prototypes to larger games.

## Example Usage

### 1. Add a new module

Create a new folder under `game/`.

Add `mod.rs`, `components.rs`, and `systems.rs`.

Register it in `game/mod.rs`.

### 2. Define components and systems

Keep each module **self-contained**.

Use `pub(super)` or `pub(crate)` visibility for tight encapsulation.

### 3. Integrate into `main.rs`

Import and add your core and game plugins.

```rust
fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_plugin(core::CorePlugin)
        .add_plugin(game::GamePlugin)
        .run();
}
```

## In Practice

This is a (somewhat loose) example structure for a Pong clone made with Bevy using the Modular Bevy Project Architecture:

```
src/
├── main.rs
├── core/
│   ├── mod.rs
│   ├── resources.rs --> game state, window size...
│   └── setup.rs --> window setup, spawn camera, UI...
└── game/
    ├── mod.rs
    ├── ball/
    │   ├── mod.rs
    │   ├── components.rs --> ball radius, velocity...
    │   ├── systems.rs --> spawn ball, update ball position...
    │   └── physics.rs --> check ball collision with paddles, window borders...
    ├── paddle/
    │   ├── mod.rs
    │   ├── components.rs --> paddle dimensions...
    │   ├── systems.rs --> spawn paddle...
    │   └── controller.rs --> player movement, enemy movement...
    ├── score/
    │   ├── mod.rs
    |   ├── resources.rs --> Score enum (left and right)...
    │   └── systems.rs --> spawn score, update score...
    └── quit/
        ├── mod.rs
        └── systems.rs --> quit on escape key...
```

## Effects Of Good Implementation

- Easy onboarding for new developers.

- Predictable structure for any feature.

- Simplified debugging and testing.

- Cleaner Git diffs and reviews.

*when using this architecture, credits are not required, but always welcomed :)*
