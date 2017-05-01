# buildpacks
Buildpacks provide **framework and runtime support** for your applications. Buildpacks typically examine user-provided artifacts to determine what dependencies to download and how to configure applications to communicate with bound services.

When you push an application, Cloud Foundry **automatically detects** which buildpack is required and installs it on the Diego cell or Droplet Execution Agent (DEA) where the application needs to run.

[https://docs.cloudfoundry.org/buildpacks/](https://docs.cloudfoundry.org/buildpacks/)

**list buildpacks:**
```shell
$ cf buildpacks
```
## specify buildpacks in manifest:
- By name: ```MY-BUILDPACK```.
- By GitHub URL: ```https://github.com/cloudfoundry/java-buildpack.git```.
- By GitHub URL with a branch or tag:
  ```https://github.com/cloudfoundry/java-buildpack.git#v3.3.0``` for the ```v3.3.0``` tag.

```yaml
---
  ...
  buildpack: buildpack_URL
```
The command line option that overrides this attribute is ```-b```.

## Buildpack Detection
**Detect buildpack one by one with positions**
During staging, each buildpack has a position in a priority list (identified by running cf buildpacks). Cloud Foundry checks if the buildpack in position 1 is a compatible buildpack. If the position 1 buildpack is not compatible, Cloud Foundry moves on to the buildpack in position 2. Cloud Foundry continues this process until the correct buildpack is found. If no buildpack is compatible cf push fails with the following error:
```shell
None of the buildpacks detected a compatible application
Exit status 222
Staging failed: Exited with status 222

FAILED
NoAppDetectedError
```

## Customizing and Creating Buildpacks
A buildpack repository contains three main scripts, situated in a folder named ```bin```.
### bin/detect
The script is called with one argument, the ```build``` directory for the app. The build directory contains the app files uploaded when a user performs a ```cf push```.

The ```detect``` script returns an exit code of ```0``` if the buildpack is compatible with the app.

```ruby
#!/usr/bin/env ruby

gemfile_path = File.join ARGV[0], "Gemfile"

if File.exist?(gemfile_path)
  puts "Ruby"
  exit 0
else
  exit 1
end
```

### bin/compile
The ```compile``` script builds a droplet by packaging the app dependencies, assuring that the app has all the necessary components needed to run.

The script is run with **two arguments**: the ```build``` directory for the app and the **cache** directory, which is a location the buildpack can use to store assets during the build process. 

```ruby
#!/usr/bin/env ruby

#sync output

$stdout.sync = true

build_path = ARGV[0]
cache_path = ARGV[1]

install_ruby

private

def install_ruby
  puts "Installing Ruby"

  # !!! build tasks go here !!!
  # download ruby 
  # install ruby
end
```

### bin/release
The ```release``` script provides feedback metadata to Cloud Foundry indicating how the app should be executed. The script is run with one argument, the ```build``` directory. **The script must generate a YAML file in the following format:**
```yaml
default_process_types:
  web: start_command.filetype
```

## Use the Buildpack Packager
1. Ensure that you have installed the buildpack packager:
  [https://github.com/cloudfoundry/buildpack-packager](https://github.com/cloudfoundry/buildpack-packager)

2. Create a manifest.yml in your buildpack.
3. Run the packager in cached mode:
```shell
$ buildpack-packager --cached
```
The packager will add (almost) everything in your buildpack directory into a zip file. It will exclude anything marked for exclusion in your manifest.

In cached mode, the packager will download and add dependencies as described in the manifest.

**manifest.yml for Buildpack Packager:**
[https://github.com/cloudfoundry/buildpack-packager](https://github.com/cloudfoundry/buildpack-packager)


## Managing Custom Buildpacks
[https://docs.cloudfoundry.org/adminguide/buildpacks.html](https://docs.cloudfoundry.org/adminguide/buildpacks.html)

## Packaging Dependencies for Offline Buildpacks
[https://docs.cloudfoundry.org/buildpacks/depend-pkg-offline.html](https://docs.cloudfoundry.org/buildpacks/depend-pkg-offline.html)
