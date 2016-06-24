Dockerfiles for compiling Maidsafe's safe_vault.

<b>Usage:</b>

Create folders to receive the built binaries:
```
$ mkdir ~/safe_vault-amd64 ~/safe_vault-armv7   # and so on, for future additions
```

Clone this repo.
```
$ cd dockerfiles

$ chmod +x build-script
```

cd into each folder and:

```
$ docker build -t ${PWD##*/} .

$ cd ..
```

Run the script:
```
$ ./build-script
```
