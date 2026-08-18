# cargo-ament-build

This cargo plugin is a wrapper around `cargo build` which installs build artifacts in a layout expected by ament and ROS 2 tools.

It can be used standalone or through `colcon-ros-cargo`. Its command line interface is `cargo ament-build --install-base <install base> -- <cargo build args>`.

What does this plugin do?
- It builds or checks the package, depending on whether it contains any binaries
- It copies the source code and binaries to appropriate locations in the install base
- It places marker files in the ament index

It is possible to specify additional files or directories to be installed in the `metadata` section of `Cargo.toml` like this:
```
[package.metadata.ros]
install_to_share = ["launch", "config"]
```
These paths are relative to the directory containing the `Cargo.toml` file and will be copied to the appropriate location in `share`.

The same mechanism applies with `install_to_include` and `install_to_lib`.

It is also possible to register arbitrary ament index resources, by
specifying the resource type and its marker content like this:
```
[package.metadata.ros.ament_index_resources]
test_resource = "test_resource/foo.yaml"
sound = { content_file = "sounds/manifest.txt" }
```
Each key is an ament resource type; each value is either the
literal marker content as a string, or a table `{ content_file = "<path>" }` naming a file
relative to the directory containing `Cargo.toml`. A
marker file is created at `share/ament_index/resource_index/<resource type>/<package_name>`
with exactly that content.
