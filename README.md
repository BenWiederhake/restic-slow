# Documentation for restic/restic#2284

- `reproduce`: Script to produce large repositories that demonstrate the buggy behavior really well.
  Takes one hour to run, produces a file like `repos-contains447MiB.tar.xz`, and a little bit more.
- `repos-contains447MiB.tar.xz`: Copies of a restic-repository undergoing expansion.
  The restic-repositories `a` through `k` gain 500 restic-snapshots in each step.
  Note that these repositories stem from each other, which is why this small 20MB
  archive manages to store 447MB worth of encrypted data.  The password is `1234`.
- `flamegraphs/perf-?-?.data.svg`: I ran `perf record restic check` on each repository five times.
  The 22 GiB output doesn't fit anywhere, but the resulting flamegraphs are small enough.  They are what helped me find the issue.
  * Caveat: d1, e1, f1, h1, and k5 are probably outliers with respect to timing.  My system is noisy, that's why I ran it 5 times each.
  * Caveat: d4, f1, f4, h2, k1, k2, k3, k4, k5 have lost some samples.  perf record couldn't keep up.
  * Caveat: Something's wonky with i3: "1 sample out of order".  perf record doesn't like go's concurrency I guess.
  * That's why I cherry-picked run 5.  There are the least problems.
- `flamegraphs/perf-more.tar.xz`: All the other runs, compressed.
