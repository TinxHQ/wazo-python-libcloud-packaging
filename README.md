# tinx-python-libcloud-packaging

Debian packaging for [libcloud](http://libcloud.apache.org/).

## Building a new version

To build a new version of libcloud.

* Set the new version in the `VERSION` file.
* Add a changelog entry entry in `debian/changelog`
* Push to the master branch

### Changelog Version Number

The version in the changelog MUST have the following format:

`<upstream_version>-1~tinx<1..n>`

## Building Locally

To build on a test environment before submitting a change to production the following procedure can be used.

```su
make -f debian/rules get-orig-source
tar -xvf ../tinx-python-libcloud-packaging_*.orig.tar.gz  --strip 1
dpkg-buildpackage -us -uc
```

The `.deb` will be located in the parent directory.
