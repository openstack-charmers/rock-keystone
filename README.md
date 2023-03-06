# Keystone ROCK

This is a ROCK OCI image for Keystone.

More information is coming.

For now, if you want to play with it:

```bash
> sudo snap install rockcraft --edge --classic
> rockcraft pack
```

Now that you have created the ROCK, you can import it into
your local docker repository. Using skopeo is a good idea as
it will help ensure that all layers of the image are imported
into docker (this is just the top layer).

```bash
> skopeo --insecure-policy copy oci-archive:keystone_yoga_amd64.rock docker-daemon:keystone:yoga
# temp fix canonical/rockcraft#208
> CTR=$(docker run --rm -d keystone:yoga sleep infinity)
> docker exec $CTR ln -srf /usr/share/perl/5.30.0 /usr/share/perl/5.30
> docker commit $CTR keystone:yoga
> docker kill $CTR
```

If you are interested in giving it a go in Microk8s, you can
export the image from your docker registry and then into the
microk8s registry:

```bash
> docker save keystone:yoga > ./keystone_yoga.tar
> microk8s ctr image import ./keystone_yoga.tar
# Try with sunbeam
> juju attach-resource keystone keystone-image=keystone:yoga
```
