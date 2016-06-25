<h2>Dockerfiles for compiling Maidsafe's safe_vault</h2>

<h3>Usage:</h3>

1. Clone this repo.

2. ``` $ cd dockerfiles```

3. Create the Docker images by cd'ing into each directory and:
   ```
   $ docker build -t ${PWD##*/} .    # a regex to get the name of the folder
   ```

4. Run the script:
   ```
   $ chmod +x build

   $ ./build
   ```

The resulting binaries will be found in directory "target".

For subsequent builds you need only run the script in order to obtain fresh binaries. The script can be run from anywhere on your system where you have write privileges.
