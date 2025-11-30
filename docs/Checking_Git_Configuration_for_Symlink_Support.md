how can I check current git config wether symlink follow or not 


**Commands**

- Check the config value (local repo):  
  `git config --get core.symlinks`  
  `git config --get core.followSymlinks`

- Show where the value is coming from (source + value):  
  `git config --show-origin --get core.symlinks`  
  `git config --show-origin --get core.followSymlinks`

- Check all scopes explicitly:  
  `git config --local --get core.symlinks`  
  `git config --global --get core.symlinks`  
  `git config --system --get core.symlinks`

- List any config entries mentioning symlink:  
  `git config --show-origin --list | rg 'symlink'`

- Inspect the index to see if a tracked path is a symlink (mode `120000`):  
  `git ls-files -s path/to/file`  (mode `120000` = symlink)

- Check the working-tree file type:  
  `test -L path/to/file && echo "is symlink" || echo "not symlink"`

**Quick interpretation**

- `core.symlinks = true` → Git will create real symlinks in the working tree.  
- `core.symlinks = false` → Git will write the link target as a normal file (no symlink).  
- `core.followSymlinks` (check it too) affects how Git treats existing symlinks in the worktree when updating files; inspect with the same `git config` commands above.

If a setting is unset, Git uses platform defaults (on Unix-like systems symlinks are normally supported). For full details see `git help config` (look up `core.symlinks` / `core.followSymlinks`).
