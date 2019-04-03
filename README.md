# Heroku buildpack for git-lfs

This is a [Heroku buildpack][buildpacks] which downloads your [Git
LFS][git-lfs] assets during deployment, which Heroku does not do by
default.

## Usage

To configure Git LFS for your Heroku app called `<myapp>`, first
install [heroku-buildpack-apt] like so:

    $ heroku buildpacks:add heroku-community/apt -a <myapp>

Create an `Aptfile` that includes `git-lfs` on a line of its own.
(That is all the file needs to contain.) Then install this buildpack
like so:

    $ heroku buildpacks:add                                   \
        https://github.com/raxod502/heroku-buildpack-git-lfs  \
        -a <myapp>

You can verify the buildpacks enabled for your app by running `heroku
buildpacks -a <myapp>`. Make sure that this buildpack is listed
*after* heroku-buildpack-apt, since Git LFS must be installed when
this buildpack runs!

Set the following environment variable for your app:

* `HEROKU_BUILDPACK_GIT_LFS_REPO` to the clone URL of the repository
  from which to download Git LFS assets. This should include any
  username, password, or personal access token which is necessary to
  clone noninteractively. See [here][noninteractive-clone] for
  details on the syntax.

After the next time you deploy your app, Git LFS assets will be
downloaded and checked out automatically.

[buildpacks]: https://devcenter.heroku.com/articles/buildpacks
[git-lfs]: https://git-lfs.github.com/
[heroku-buildpack-apt]: https://github.com/heroku/heroku-buildpack-apt
[noninteractive-clone]: https://stackoverflow.com/q/10054318/3538165
