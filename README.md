veristat
[![Build](https://github.com/libbpf/veristat/actions/workflows/build.yml/badge.svg)](https://github.com/libbpf/veristat/actions/workflows/build.yml)
========

> This is a mirror of veristat sources from
> [bpf-next](https://kernel.googlesource.com/pub/scm/linux/kernel/git/bpf/bpf-next)
> Linux kernel tree. Veristat is located under `tools/testing/selftests/bpf`
> directory.

veristat is the tool for loading, verifying, and debugging BPF object files.
It allows to work with BPF object files convenient and quickly, without having
to use or modify corresponding user-space parts of an application.

We currently don't have a good documentation, so please refer to commit
messages explaining each feature as they were added to veristat upstream:

  - general loading functionality ([patch](https://patchwork.kernel.org/project/netdevbpf/patch/20220909193053.577111-4-andrii@kernel.org/));
  - comparison mode ([patch](https://patchwork.kernel.org/project/netdevbpf/patch/20220921164254.3630690-4-andrii@kernel.org/));
  - results replay mode ([patch](https://patchwork.kernel.org/project/netdevbpf/patch/20221103055304.2904589-2-andrii@kernel.org/));
  - filtering ([patch](https://patchwork.kernel.org/project/netdevbpf/patch/20220921164254.3630690-5-andrii@kernel.org/),
               [patch](https://patchwork.kernel.org/project/netdevbpf/patch/20221103055304.2904589-11-andrii@kernel.org/));
  - stats ordering ([patch](https://patchwork.kernel.org/project/netdevbpf/patch/20221103055304.2904589-10-andrii@kernel.org/));
  - controlling BPF verifier log verboseness ([patch](https://patchwork.kernel.org/project/netdevbpf/patch/20220923175913.3272430-6-andrii@kernel.org/)).

Over time we hopefully will get a more usable and consolidated documentation.
