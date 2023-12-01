# rules_oci bugs

## Windows

- Running from drive different than your `%USERPROFILE%` one (typically `C:`), for example on `D:\` would try to read `\Users\<username>\.docker\config.json` (I've replaced my username with `<username>` below)

        ERROR: <builtin>: fetching oci_alias rule //:rules_oci~1.4.3~oci~distroless_base: Traceback (most recent call last):
                File "C:/users/<username>/_bazel_<username>/roe7hhvn/external/rules_oci~1.4.3/oci/private/pull.bzl", line 472, column 36, in _oci_alias_impl
                        downloader = _create_downloader(rctx)
                File "C:/users/<username>/_bazel_<username>/roe7hhvn/external/rules_oci~1.4.3/oci/private/pull.bzl", line 318, column 40, in _create_downloader
                        "config": _get_auth_config_path(rctx),
                File "C:/users/<username>/_bazel_<username>/roe7hhvn/external/rules_oci~1.4.3/oci/private/pull.bzl", line 312, column 37, in _get_auth_config_path
                        return json.decode(rctx.read(path))
        Error in read: java.io.FileNotFoundException: \Users\<username>\.docker\config.json (The system cannot find the path specified)
        ERROR: java.io.FileNotFoundException: \Users\<username>\.docker\config.json (The system cannot find the path specified)
        
- Workaround
  * Not best workaround, but setting `DOCKER_CONFIG` to some value in `.bazelrc` _"fixes"_ this
  
        common --repo_env=DOCKER_CONFIG=does_not_matter

  * It reports now:
  
        WARNING: Could not find the `$HOME/.docker/config.json` and `$XDG_RUNTIME_DIR/containers/auth.json` file. Running one of `podman login`, `docker login`, `crane login` may help.
        

But lets you go through :), though probably without authorization
