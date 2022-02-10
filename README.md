# Heroku buildpack for git-lfs

This is a [Heroku buildpack][buildpacks] which installs [Git
LFS][git-lfs] and downloads your Git LFS assets during deployment
(which Heroku does not do by default).

## Usage

To configure Git LFS for your Heroku app called `<myapp>`, run:

    $ heroku buildpacks:add                                   \
        https://github.com/raxod502/heroku-buildpack-git-lfs  \
        -a <myapp>

Set the following environment variable (i.e. using `heroku
config:set`) for your app:

* `BUILDPACK_GIT_LFS_REPO` to the clone URL of the repository from
  which to download Git LFS assets. This should include any username,
  password, or personal access token which is necessary to clone
  noninteractively. See [here][noninteractive-clone] for details on
  the syntax.

After the next time you deploy your app, Git LFS assets will be
downloaded and checked out automatically, and Git LFS will be
available on `PATH` for your app.

## Tips

### Pushing to Heroku from Git

If you are using Git LFS and want to deploy to Heroku using Git, make
sure to provide the `--no-verify` flag to `git push`, since Heroku's
Git server will reject LFS assets and this will cause an error
otherwise.

### Slug size too large

If your LFS assets are around 500 MB or more, you won't be able to use
this buildpack. In fact, you won't be able to use *any* buildpack to
get things working---Heroku simply has a max 500 MB size limit on the
assets that are deployed as part of your application, and there is no
way around this.

If you absolutely must deploy such large assets on Heroku, the only
way to do it is to download them at runtime, *after* your application
starts up. What you need to do is configure your server to trigger the
downloads at startup, in the background, and successfully serve
requests while that is happening (if the server hasn't bound to its
port quickly enough then Heroku will kill and restart it).

This will require some substantial custom scripting knowledge specific
to your application and that is unfortunately not something this
buildpack can assist you with.

You may want to reconsider your architecture and/or deployment target
if you find yourself in this situation.

[buildpacks]: https://devcenter.heroku.com/articles/buildpacks
[git-lfs]: https://git-lfs.github.com/
[heroku-buildpack-apt]: https://github.com/heroku/heroku-buildpack-apt
[noninteractive-clone]: https://stackoverflow.com/a/50193010/3538165
