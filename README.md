# Heroku buildpack for git-lfs

This is a [Heroku buildpack][buildpacks] which installs [Git
LFS][git-lfs] and downloads your Git LFS assets during deployment
(which Heroku does not do by default).

## Usage

To configure Git LFS for your Heroku app called `<myapp>`, run:

    $ heroku buildpacks:add                                   \
        https://github.com/raxod502/heroku-buildpack-git-lfs  \
        -a <myapp>

Set the following environment variable for your app:

* `HEROKU_BUILDPACK_GIT_LFS_REPO` to the clone URL of the repository
  from which to download Git LFS assets. This should include any
  username, password, or personal access token which is necessary to
  clone noninteractively. See [here][noninteractive-clone] for
  details on the syntax.

After the next time you deploy your app, Git LFS assets will be
downloaded and checked out automatically, and Git LFS will be
available on `PATH` for your app.

## Tips

If you are using Git LFS and want to deploy to Heroku using Git, make
sure to provide the `--no-verify` flag to `git push`, since Heroku's
Git server will reject LFS assets and this will cause an error
otherwise.

[buildpacks]: https://devcenter.heroku.com/articles/buildpacks
[git-lfs]: https://git-lfs.github.com/
[heroku-buildpack-apt]: https://github.com/heroku/heroku-buildpack-apt
[noninteractive-clone]: https://stackoverflow.com/a/50193010/3538165
