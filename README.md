# NGC Replicator

Clones nvcr.io using the either DGX (compute.nvidia.com) or NGC (ngc.nvidia.com)
API keys.

The replicator will make an offline clone of the NGC/DGX container registry.
In its current form, the replicator will download every CUDA container image as
well as each Deep Learning framework image in the NVIDIA project.

Tarfiles will be saved in `/output` inside the container, so be sure to volume
mount that directory. In the following example, we will collect our images in
`/tmp` on the host.

Use `--min-version` to limit the number of versions to download.  In the example
below, we will only clone versions `18.04` and later DL framework images.

```
docker run --rm -it -v /var/run/docker.sock:/var/run/docker.sock -v /tmp:/output \
    deepops/replicator --project=nvidia --min-version=18.04 \
                       --api-key=<your-dgx-or-ngc-api-key>
```

You can also filter on specific images.  If you only wanted Tensorflow, PyTorch
and TensorRT, you would simply add `--image` for each option, e.g.

```
docker run --rm -it -v /var/run/docker.sock:/var/run/docker.sock -v /tmp:/output \
    deepops/replicator --project=nvidia --min-version=18.04 \
                       --image=tensorflow --image=pytorch --image=tensorrt \
                       --dry-run \
                       --api-key=<your-dgx-or-ngc-api-key>
```

Note: the `--dry-run` option lets you see what will happen without committing
to a lengthy download.

Note: a `state.yml` file will be created the output directory.  This saved state will be used to
avoid pulling images that were previously pulled.  If you wish to repull and save an image, just
delete the entry in `state.yml` corresponding to the `image_name` and `tag` you wish to refresh.

## Developer Quickstart

```
cd python && make
cd replicator
make dev
py.test
```

## TODOs

- [x] save markdown readmes for each image.  these are not version controlled
- [ ] test local registry push service.  coded, but not tested
- [ ] add templater to workflow
