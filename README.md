# ghidra-wolfi
Ghidra built for Wolfi + Apko

This is an experimental build of Ghidra as a Wolfi package and accompanying Apko image.

This process results in a functional version of Ghidra with minimal CVEs.

Build process:
1. `melange keygen`
2. `melange build --arch x86_64 ghidra.yaml --signing-key melange.rsa`
3. `apko build ghidra.apko.yaml ghidra:latest ghidra.tar`
4. `docker load < ghidra.tar`
5. `xhost +local:@docker; docker run --rm -it --net=host -e DISPLAY=host.docker.internal:0 -v /tmp/.X11-unix/:/tmp/.X11-unix/ ghidra:latest-amd64`

The image is built with `DISPLAY=:0` but may need to be overridden depending on the OS. For macOS, [XQuartz](https://www.xquartz.org/) is required; `xhost + 127.0.0.1` may also be required.

When the image is run, the following console output is expected:
```
non-network local connections being added to access control list
WARNING: The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8) and no specific platform was requested
Picked up _JAVA_OPTIONS: -Dsun.java2d.xrender=false
Picked up _JAVA_OPTIONS: -Dsun.java2d.xrender=false
```
after which the Ghidra UI will be visible. The container uses `tail -f /dev/null` to run indefinitely.
