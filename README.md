# Cloud Foundry - Light Release example

This repository acts as example of an idea of a "light release". The minimum set of YAML files that are required to upload the Cloud Foundry Bosh release to a target bosh.

Currently, as at the start of 2013, the documented methods for getting cf-release into a bosh is either:

```
git clone https://github.com/cloudfoundry/cf-release.git
# downloads a huge git repo and submodules
cd cf-release
./update            # downloads all blobs locally
bosh create release # create a dev release
bosh upload release # uploads the dev release
```


Or the more efficient method via final releases:

```
git clone https://github.com/cloudfoundry/cf-release.git
# downloads a huge git repo and submodules
cd cf-release
bosh upload release releases/appcloud-131.yml # downloads public blobs, uploads release
```

In both cases you have 1.5G of blobs to download to your laptop and then upload to your Bosh director.

How much of cf-release is really required to perform the `bosh upload release` command above? All of it? Some files?

It turns out we only need the following files/folders. This repository is an example of these minimal set of files.

```
.final_builds
config
releases/appcloud-XYZ.yml
```

Due to current validations within the bosh cli, we also need three blank folders: `src`, `jobs`, `packages`.

Want to see if it works?

```
git clone https://github.com/StarkAndWayne/cf-release-only.git
cd cf-release-only
bosh upload release releases/appcloud-131.yml
```

This is the idea of a "light release".

What if this repository represented a small bundle of files that could be uploaded directly to Bosh, or that bosh could download directly (or via cf-release itself), and then the Bosh director did all the work of downloading the blobs.

You would never have to download a git repo, never have to download 1.5G of cf-release blobs, never have to then upload them again to your bosh. You could just give the cf-release URL to your bosh and say "go and install the release from there."

## Thanks

Thanks to Martin Englund for also having similar product ideas for the future of Bosh. Martin suggests the following CLI command:

```
bosh add remote release https://github.com/cloudfoundry/cf-release.git
```

Your bosh would automatically discover the `appcloud` release and import the latest release within it (`appcloud-131.yml` at the time of writing).

Thanks to Elizabeth Hendrickson who let me show her the delights of final releases vs dev releases; which prompted the question "what is the smallest set of files needed to upload a release?" which resulted in this small repo!
