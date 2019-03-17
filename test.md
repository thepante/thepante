### Yeah!  

Just have to add the other origin from were it was cloned:
 - Clone a repo, then add a origin
 - If cloned from GitHub, then add url origin of repo on GitLab
 - If cloned from GitLab, add url origin of repo on GitHub

```bash
git remote set-url ––add origin https://github.com/thepante/repo.git
```
  
Can verify in `.git/config` under `[remote "origin"]`
