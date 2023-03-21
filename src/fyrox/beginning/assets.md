# Assets

Pretty much every game depends on various assets, such as 3D models, textures, sounds, etc. Fyrox has its own 
assets pipeline made to make your life easier.

## Asset Types

The engine offers a set of assets that should cover all of your needs:

- [Models](../resources/model.md) - are a set of objects. They can be a simple 3D model (barrels, bushes, weapons, 
etc.) or complex scenes with lots of objects and possibly other model instances. Fyrox supports two main formats:
FBX - which can be used to import 3D models, RGS - which are scenes made in Fyroxed. RGS models are special, as they can be 
used as _hierarchical prefabs_.
- [Textures](../resources/texture.md) - are images used to add graphical details to objects. The
engine supports multiple texture formats, such as PNG, JPG, BMP, etc. Compressed textures in DDS format are also supported.
- [Sound buffers](../resources/sound.md) - are data buffers for sound sources. Fyrox supports WAV and OGG formats. 
- [Curves](../resources/curve.md) - are parametric curves. They're used to create complex functions for numeric parameters.
They can be made in the `Curve Editor` (`Utils -> Curve Editor`)

## Asset Management

Asset management is performed from the `Asset Browser` window in the editor, you can select an asset, preview it, and edit
its import options. Here's a screenshot of the asset browser with a texture selected:

![asset browser](assets.png)

The most interesting part here is the import options section under the previewer. It allows you to set asset-specific import options
and apply them. Every asset has its own set of import options. See their respective asset page from the section above to learn
what each import option is for.

## Asset Instantiation

Some asset types can be instantiated in scenes; for now, you can only create direct instances from models. This
is done by simply dragging the model you want to instantiate and dropping it on the `Scene Preview`. While dragging it, you'll also see a preview of the model.

![preview](preview.gif)

The maximum amount of asset instances is not limited by the engine but it is by the memory and CPU resources of your PC. 
Note that the engine does try to reuse data across instances as much as possible.

You can also instantiate assets dynamically from your code. Here's an example of that for a Model:

```rust,no_run,edition2018
# extern crate fyrox;
# use fyrox::{
#     core::pool::Handle,
#     engine::resource_manager::ResourceManager,
#     scene::{node::Node, Scene},
# };
# use std::path::Path;
async fn instantiate_model(
    path: &Path,
    resource_manager: ResourceManager,
    scene: &mut Scene,
) -> Handle<Node> {
    // Load the model first. Alternatively, you can store the resource handle somewhere and use it for instantiation.
    let model = resource_manager.request_model(path).await.unwrap();

    model.instantiate(scene)
}
```

This is very useful with prefabs that you may want to instantiate in a scene at runtime. 

## Loading Assets

Usually, there is no need to manually handle the loading of assets since you have the editor to help with that - just create
a scene with all the required assets. However, there are times when you may need to instantiate some asset dynamically, for 
example, a bot prefab. For this cases, you can use the `ResourceManager` and its respective methods, such as `request_model`, 
`request_texture`, etc.