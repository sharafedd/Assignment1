FROM ubuntu:24.04

# Runtime environment settings
ENV AFL_SKIP_CPUFREQ=0
ENV AFL_I_DONT_CARE_ABOUT_MISSING_CRASHES=1

# Copying in the target program source code
RUN mkdir /target
COPY js.c /target

# DONE: Built AFL++ and compiled js.c for fuzzing.

# 1. Install software required to compile AFL++ and js.c
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    clang \
    llvm \
    llvm-dev \
    lld \
    git \
    make \
    ca-certificates \
 && rm -rf /var/lib/apt/lists/*

# 2. Clone and build AFL++
RUN git clone https://github.com/AFLplusplus/AFLplusplus /AFLplusplus
WORKDIR /AFLplusplus
RUN make source-only 
RUN make install

# 3. Compile js.c
WORKDIR /target
RUN afl-clang-fast -g -O2 -o js js.c

# 4. Copy in/seed
COPY in/ /target/in/

# 5. Run run.sh
# CMD ["/bin/bash"]

