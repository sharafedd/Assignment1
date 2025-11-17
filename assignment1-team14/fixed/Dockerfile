FROM ubuntu:24.04

# Copying in the target program source code
RUN mkdir /target
COPY js.c /target

# 1. Install all dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    clang \
    patch \
    build-essential \
 && rm -rf /var/lib/apt/lists/*

# 2. Create working directory
WORKDIR /target

# 3. Copy required files from your submission folder into container
COPY js.patch js.patch
COPY crash000000.txt crash000000.txt

# 4. Apply the patch
RUN echo "Applying patch..." \
 && patch < js.patch \
 && echo "[OK] Patch applied successfully."

# 5. Compile the patched js.c
RUN echo "Compiling program..." \
 && clang -g js.c -o js \
 && echo "[OK] Compilation successful."

# 6. Run the program on the crashing test to prove it is fixed
CMD ["/bin/sh", "-c", "echo 'Running patched program...'; ./js crash000000.txt"]
