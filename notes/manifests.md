## Manifests
This includes everything from how many instances to create and how much memory to allocate to what services applications should use.

**A manifest can help you automate deployment, especially of multiple applications at once.**

- Manifests are written in YAML.
- The manifest may begin with three dashes.
- The applications block begins with a heading followed by a colon.
- The application name is preceded by a single dash and one space.
- Subsequent lines in the block are indented two spaces to align with name.

Example:
```yaml
---
applications:
- name: nifty-gui
  memory: 512M
  host: nifty
```
**Notes:**
- A minimal manifest requires only an application name.
If you do not use a manifest, the minimal push command looks like this:
```shell
$ cf push my-app
```
- Manifests override most recent values, including defaults.
- Command line options override manifests.

Example:
the ```cf scale``` command changes the number of instances.(override manifests.)

### Environment Variables
```yaml
---
  ...
  env:
    RAILS_ENV: production
    RACK_ENV: production
```
### Services
```yaml
---
  ...
  services:
   - instance_ABC
   - instance_XYZ
```
### Deploy two or more apps described in manifest
```yaml
---
  ...
# all applications use these settings and services
domain: shared-domain.example.com
memory: 1G
instances: 1
services:
- clockwork-mysql
applications:
- name: springtock
  host: tock09876
  path: ./spring-music/build/libs/spring-music.war
- name: springtick
  host: tick09875
  path: ./spring-music/build/libs/spring-music.war
```

### Optional Attributes
- buildpack
- command(**custom command to start an application**,set ```null``` to force use the buildpack command) 
- disk_quota
- domain
- domains
- health-check-http-endpoint
- health-check-type
- host
- hosts
- instances
- memory
- no-hostname
- no-route
- path
- random-route
- routes
- stack
- timeout

Refer: [https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html)

