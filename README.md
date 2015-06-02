# Body Labs mesh rigger

This library enables developers to automatically rig any mesh generated by the
[Body Labs Instant API][mesh-docs]. The rig is [Mixamo][mixamo]-compatible,
allowing you to easily [create control rigs][mixamo-scripts] and apply motion
capture files provided by Mixamo.

```python
from bodylabs_rigger.factory import RiggedModelFactory
from bodylabs_rigger.fbx_util import (
    create_fbx_manager,
    export_fbx_scene,
)

# Create a rigging factory.
factory = RiggedModelFactory.create_default()

fbx_manager = create_fbx_manager()

# Rig and export a mesh (which is a numpy Vx3 array of vertices).
rigged_mesh = factory.construct_rig(mesh_vertices, fbx_manager)
export_fbx_scene(fbx_manager, rigged_mesh, output_path)

# Clean up Fbx objects.
rigged_mesh.Destroy()
fbx_manager.Destroy()
```

We also provide a `MeshGenerator` library, which allows you to create
riggable meshes from body measurements using [BodyKit][bodykit].

```python
import os
from bodylabs_rigger.bodykit.mesh_generator import MeshGenerator

# Make sure to set BODYKIT_ACCESS_KEY and BODYKIT_SECRET
# in your execution environment.
mesh_generator = MeshGenerator(
    os.environ['BODYKIT_ACCESS_KEY'],
    os.environ['BODYKIT_SECRET']
)

mesh = mesh_generator.get_mesh_for_measurements(
    {'height': 70, 'weight': 150},
    unit_system='unitedStates',
    gender='male'
)

rigged_mesh = factory.construct_rig(mesh, fbx_manager)
```

[`examples/meshes_from_bodykit.py`][example-script] puts all the pieces
together to randomly generate and rig a set of meshes.

```
python examples/meshes_from_bodykit.py \
    ~/Desktop/bodylabs_rig_examples \
    --num_meshes 5
```

[mesh-docs]: http://developer.bodylabs.com/instant_api_reference.html#Mesh
[mixamo]: https://www.mixamo.com/
[mixamo-scripts]: https://www.mixamo.com/scripts
[bodykit]: http://www.bodylabs.com/bodykit.html
[example-script]: https://github.com/bodylabs/rigger/blob/master/examples/meshes_from_bodykit.py

## Installation

1. Install [Python FBX][python-fbx]
2. `pip install -e git+https://github.com/bodylabs/rigger.git#egg=bodylabs_rigger`

This library has been tested on Mac OS X and Linux with Python 2.7.
Windows is not officially supported at this time.

[python-fbx]: http://help.autodesk.com/view/FBX/2015/ENU/?guid=__files_GUID_2F3A42FA_4C19_42F2_BC4F_B9EC64EA16AA_htm

## Contribute

* Issue tracker: http://github.com/bodylabs/rigger/issues
* Source code: http://github.com/bodylabs/rigger

## Support

If you have questions or run into any issues, please contact us via the
[BodyKit developer support page][bodykit-support].

[bodykit-support]: http://developer.bodylabs.com/help_and_support.html

## License

This project is licensed under the BSD license.
