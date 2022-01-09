# hacking a pub.dev repo for fun

`Dart` & `Flutter` is a little bit like `ruby on rails`. Language is connected to the main framework in a way.
Like each eco-system those days, dart has its own libraries and package repository.
Substitute of `pypi` or `runygems` in dart's world is called `pub.dev` and `pip` or `gem` is `pub`.


So let's focus on those 2 components. By creating a package in dart, we are basically creating a yaml file with some metadata. The file is called `pubspec.yaml` and within the source code is compressed to a `tar.gz` archive form. The next step is to upload a package to the `pub.dev` system, which will analyze it, generate some scoring and stats. Once done, `pub.dev` will serve the package.

## Scenario nr 1

What will happen if `pubspec.yaml` contains recursive references (`yamlbomb`)? As usually executing a DoS attack on someone's else servers is not covered by a bug-bounty program and collecting clear metrics is not the most pleasant process, I decided to replicate a setup. After a few evenings spent discovering things that should be documented, finally I got working `pub.dev` on my own infrastructure. Drums... & `htop` shows `CPU` utilization 100% and everything is slowing down. BINGO!

## Scenario nr 2

What if `pubspec` will contain a `<script>alert(document.domain)</script>` as a `homepage`? The client (`pub`) says `"No!"`. Small patch on `pub`, recompile & retry. Bingo... ? Partly... The server has accepted the rotten package without any issue, but `CSP` protects users from real dramas. What if `JS` gets replaced with `iframe`? Bingo! Never gonna give you up...

## Scenario nr 3

Assuming injection to the main package site was successful, we can try to do the same via docs (docstrings). Effect? Same as previously, `CSP` prevents tragedy, but a YouTube video will document method `foo` in the best possible way.

## Scenario nr 4

Assuming the server is reading a `tar` archive and parses `pubspec.yaml`, maybe we will be able to trick the server again.
Unfortunately talking to the server using `curl` is not the most pleasant experience, so let's look at the `pub` client again. The poisoned archive contains a few `pubspec.yaml`, but the server is properly validating each of them. Wait a second, the `pub` source code is performing a size check before the upload and raises an error if applicable. The server doesn't. New patch, recompile & test... Bingo - size check bypass.

## Additional Scenario nr 5

If the user decides to execute `rm -fr /` and some files will be lost, we can't blame `coreutils` developers for the loss.

Let's stop for a second and discuss how `pub.dev` works. Once fetching a `foo` package, `pub` is making a `GET` call and reads a `json`. The `json` contains specific package versions, and once the explicit version is known, `pub` is makes 2nd call to fetch the package version. Under the hood, the package is stored in a `gcs` bucket. Because all packages are already public, so the `gcs` is.

```sh
gsutil rsync gs://${SRC_BUCKET} ${DST_DIR}
```

A few hundred gigabytes & a few days later we have access to all existing packages from the repository.

So what is inside? The `tar -tvf` is giving some hints. Even if most of `ssh keys` or `certificates` are located in the `test` directories, there are few mates who have attached more than they should to the packages.

Fortunately, the issue seems to be solved [here](https://dart.dev/tools/pub/pubspec#false_secrets).

## Summary

Issues described in scenarios 1-4 were reported by the google bug-bounty program, the issue described in scenario 5 via email to project maintainers.

Marcin `<n0npax>` Niemira