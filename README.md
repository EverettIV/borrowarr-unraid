# Community Apps catalog source

Copy **everything in this folder** to the **root** of a new public GitHub repository (e.g. `borrowarr-unraid`) before submitting to [Community Apps](https://ca.unraid.net/submit/new).

Required layout after copy:

```
borrowarr-unraid/
  LICENSE
  ca_profile.xml
  icon.png
  templates/borrowarr.xml
  README.md
```

Update `templates/borrowarr.xml` with your Docker Hub image name and the correct `TemplateURL` for that repository.

See [../README.md](../README.md) for Docker Hub build and full Unraid notes.