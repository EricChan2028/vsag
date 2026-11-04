# `profile_dataset` example outputs

Example outputs of the `profile_dataset` tool proposed for
[antgroup/vsag#2862](https://github.com/antgroup/vsag/issues/2862). This branch holds generated
reports only, so that the pull request itself carries no generated files.

Download an `.html` file and open it in a browser. The pages are self-contained: their
Content-Security-Policy blocks every network request.

| Dataset | Train x dim | Metric | HTML | JSON |
| --- | --- | --- | --- | --- |
| `sift-128-euclidean` | 1,000,000 x 128 | l2 | [HTML](sift-128-euclidean.profile.html) | [JSON](sift-128-euclidean.profile.json) |
| `glove-100-angular` | 1,183,514 x 100 | cosine | [HTML](glove-100-angular.profile.html) | [JSON](glove-100-angular.profile.json) |
| `fashion-mnist-784-euclidean` | 60,000 x 784 | l2 | [HTML](fashion-mnist-784-euclidean.profile.html) | [JSON](fashion-mnist-784-euclidean.profile.json) |
| `sparse_fixture` (synthetic) | 20,000 records | ip | [HTML](sparse_fixture.profile.html) | [JSON](sparse_fixture.profile.json) |

Other files:

- [`sift-ai-request.json`](sift-ai-request.json): the exact AI request body built for SIFT1M with
  `--ai-dry-run`. Nothing was sent. It holds aggregate metrics only (no vectors, row ids or paths).
- [`screenshots/`](screenshots): the SIFT1M and GloVe reports as rendered in a browser.

## Reproduce

```bash
VSAG_ENABLE_TOOLS=ON make release
mkdir -p /tmp/data
wget http://ann-benchmarks.com/sift-128-euclidean.hdf5 -O /tmp/data/sift-128-euclidean.hdf5
./build-release/tools/profile_dataset/profile_dataset -d /tmp/data/sift-128-euclidean.hdf5
```

The default seed is 42. Reports differ only in the `run` section (time, duration, threads, peak
memory), and `determinism.metrics_sha256` stays the same for any thread count or chunk size.
The `sparse_fixture` file is synthetic: 20,000 Zipf-distributed records over a 30,000-term
vocabulary with 500 queries and exact ground truth.

Generated with commit `d9d62cefa3b6`, GCC 13.4, release build.
