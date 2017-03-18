# Git

## Tips

#### Overwriting My Local Branch with Remote Branch
```
git checkout branch
git fetch --all
git reset --hard origin/branch
```

* `git clean -d -f` clears untracked directories and files present.
* You can use `--allow-unrelated-histories` to force the merge to happen.

#### Lock file
https://github.com/blog/2328-git-lfs-2-0-0-released

```
git lfs track "*.tga" --lockable
git lfs lock foo.tga
git lfs unlock foo.tga
```
