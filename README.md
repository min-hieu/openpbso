# openpbso - An open-source library for physics-based sound

openpbso is a C++ open-source library for physics-based sound. We provide
functionalities to synthesize rigid-body sound models such as those built by the
[KleinPAT algorithm](https://graphics.stanford.edu/projects/kleinpat/) (*KleinPAT preprocessing tools not included*).

## Dependencies

Below is a list of dependencies along with the version that are tested:
* [CMake](https://cmake.org/): 3.20
* [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page): 3.2.10
* [Libigl](http://libigl.github.io/libigl/): 2.5 **(unmodified)**
* [PortAudio](http://www.portaudio.com/): (2.1)
  [here](http://www.portaudio.com/archives/pa_stable_v190600_20161030.tgz)
* [Protobuf](https://developers.google.com/protocol-buffers): 3.7.1

on MacOS, you can install via Homebrew 
```
brew install portaudio eigen3 protobuf@3 cmake
```

## Build

Since the code uses serialized data, run the protocol buffer compiler at the root directory:

    protoc --cpp_out=. ./ffat_map.proto

Build and make this project using the standard cmake routine:

    mkdir build && cd build && cmake ..
    make

This will create binaries named `real_time_modal_sound_bin` and `render_fields_bin`.

## Required runtime data

The modal sound synthesizer uses preprocessed data to run the modal sound
synthesis. You will need the following data files.
* .obj file
* surface modes file
* modal material txt file
* FFAT maps folder built by KleinPAT

## Running the synthesizer

There are two ways of loading the data files. If everything is named properly
like the [examples we provided](https://graphics.stanford.edu/projects/kleinpat/kleinpat-dataset/dataset_table.html), you can simply run

    ./real_time_modal_sound_bin -d <data_folder> -name <obj_name>

An example of `<obj_name>` would be `wine`. The `name` flag is optional.
`<data_folder>` is where the .obj file can be located.

Alternatively, you can also specify each required files/folder:

    ./real_time_modal_sound_bin -m <obj_file> -s <modes_file> -t <material_file> -p <ffat_maps_folder>

### Full example

1. [Build](#build) the `real_time_modal_sound_bin` binary.
1. Download an object sound model from the [KleinPAT dataset](https://graphics.stanford.edu/projects/kleinpat/kleinpat-dataset/dataset_table.html), and unzip the directory.
For this example, we will use the wine glass, which is called "00000" in this dataset.
This example uses the `model_00000.zip` model, which is pre-downloaded in `model/model_00000.zip`.
This zip file can be downloaded [here](https://graphics.stanford.edu/projects/kleinpat/kleinpat-dataset/data/release/v0.1/model_00000/model_00000.zip).
1. Unzip the model.
This example assumes you have unzipped `model/model_00000.zip` to `model/model_00000/`
1. Run:
    ```shell
    cd build
    ./real_time_modal_sound_bin -d ../model/model_00000 -name 00000
    ```
1. You should see the wine glass loaded in the viewer.
Use `shift+click` anywhere on the object to trigger an impact force at the clicked surface triangle.
